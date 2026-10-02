# Incident Responder — extended playbook

> Merged from [The Agency](https://github.com/msitarzewski/agency-agents) agent `security/security-incident-responder.md` (MIT License, © AgentLand Contributors).
> Only sections that add something this skill did not already cover are kept; persona, generic metrics and duplicate material were removed.

### Incident Triage & Classification

- Rapidly assess the scope, severity, and blast radius of security incidents within the first 30 minutes
- Classify incidents using a standardized severity framework: SEV1 (active data exfiltration) through SEV4 (policy violation)
- Determine whether the incident is active (attacker still present), contained, or historical
- Identify the initial access vector and determine if other systems are compromised through the same path
- **Default requirement**: Every triage decision must be documented with timestamp, evidence, and rationale — your incident timeline is both an investigation tool and a legal record

### Containment & Eradication

- Execute containment actions that stop the spread without destroying evidence — isolate, do not wipe
- Coordinate with IT operations to implement network segmentation, account lockouts, and firewall rules during active incidents
- Identify all persistence mechanisms the attacker has established: scheduled tasks, registry keys, web shells, backdoor accounts, implants
- Eradicate the threat completely — partial cleanup means the attacker returns through the mechanism you missed

### Digital Forensics & Evidence Preservation

- Acquire forensic images of compromised systems using write-blockers and validated tools — chain of custody is non-negotiable
- Analyze memory dumps for running processes, injected code, network connections, and encryption keys
- Reconstruct attacker timelines from event logs, file system timestamps, network flows, and application logs
- Correlate indicators of compromise (IOCs) across the environment to determine the full scope of the breach

### Post-Incident Recovery & Lessons Learned

- Develop recovery plans that restore business operations while maintaining security — never rush back to a compromised state
- Write post-mortem reports that distinguish root cause from contributing factors and proximate triggers
- Recommend specific, prioritized improvements — not a 50-item wish list, but the 3-5 changes that would have prevented or detected this incident
- Track remediation to completion — a finding without a fix date and owner is just a document

### Evidence Handling

- Never modify, delete, or overwrite potential evidence — forensic integrity is paramount
- Always create forensic copies before analysis — work on the copy, preserve the original
- Document the chain of custody for every piece of evidence: who collected it, when, how, and where it is stored
- Timestamp everything in UTC — timezone confusion has derailed investigations
- Preserve volatile evidence first: memory, network connections, running processes — they disappear on reboot

### Investigation Integrity

- Never assume you have found the root cause until you can explain the complete attack chain from initial access to impact
- Never attribute an attack to a specific threat actor without high-confidence technical evidence — attribution is hard and gets harder with false flags
- Always consider that the attacker may still be present and monitoring your response communications
- Verify containment actions actually worked — check for backup C2 channels, alternative persistence, and lateral movement after containment

### Windows Forensic Triage Script

```powershell
# Windows Incident Response Triage Collection
# Run as Administrator on suspected compromised system
# Collects volatile data FIRST (memory, connections, processes)

$timestamp = Get-Date -Format "yyyyMMdd-HHmmss"
$outDir = "C:\IR-Triage-$timestamp"
New-Item -ItemType Directory -Path $outDir -Force | Out-Null

Write-Host "[*] Starting IR triage collection at $timestamp (UTC: $(Get-Date -Format u))"

# === VOLATILE DATA (collect first — disappears on reboot) ===

Write-Host "[1/8] Capturing running processes with command lines..."
Get-CimInstance Win32_Process |
    Select-Object ProcessId, ParentProcessId, Name, CommandLine,
        ExecutablePath, CreationDate, @{N='Owner';E={
            $owner = Invoke-CimMethod -InputObject $_ -MethodName GetOwner
            "$($owner.Domain)\$($owner.User)"
        }} |
    Export-Csv "$outDir\processes.csv" -NoTypeInformation

Write-Host "[2/8] Capturing network connections..."
Get-NetTCPConnection |
    Select-Object LocalAddress, LocalPort, RemoteAddress, RemotePort,
        State, OwningProcess, CreationTime,
        @{N='ProcessName';E={(Get-Process -Id $_.OwningProcess -ErrorAction SilentlyContinue).ProcessName}} |
    Export-Csv "$outDir\network-connections.csv" -NoTypeInformation

Write-Host "[3/8] Capturing DNS cache..."
Get-DnsClientCache |
    Export-Csv "$outDir\dns-cache.csv" -NoTypeInformation

Write-Host "[4/8] Capturing logged-on users and sessions..."
query user 2>$null | Out-File "$outDir\logged-on-users.txt"
Get-CimInstance Win32_LogonSession |
    Export-Csv "$outDir\logon-sessions.csv" -NoTypeInformation

# === PERSISTENCE MECHANISMS ===

Write-Host "[5/8] Enumerating persistence mechanisms..."
# Scheduled tasks
Get-ScheduledTask | Where-Object { $_.State -ne 'Disabled' } |
    Select-Object TaskName, TaskPath, State,
        @{N='Actions';E={($_.Actions | ForEach-Object { $_.Execute + ' ' + $_.Arguments }) -join '; '}} |
    Export-Csv "$outDir\scheduled-tasks.csv" -NoTypeInformation

# Startup items (Run keys)
$runKeys = @(
    "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Run",
    "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce",
    "HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Run",
    "HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce"
)
$runKeys | ForEach-Object {
    if (Test-Path $_) {
        Get-ItemProperty $_ | Select-Object PSPath, * -ExcludeProperty PS*
    }
} | Export-Csv "$outDir\run-keys.csv" -NoTypeInformation

# Services (focus on non-Microsoft)
Get-CimInstance Win32_Service |
    Where-Object { $_.PathName -notlike "*\Windows\*" } |
    Select-Object Name, DisplayName, State, StartMode, PathName, StartName |
    Export-Csv "$outDir\suspicious-services.csv" -NoTypeInformation

# WMI event subscriptions (common persistence mechanism)
Get-CimInstance -Namespace root/subscription -ClassName __EventFilter 2>$null |
    Export-Csv "$outDir\wmi-event-filters.csv" -NoTypeInformation
Get-CimInstance -Namespace root/subscription -ClassName CommandLineEventConsumer 2>$null |
    Export-Csv "$outDir\wmi-consumers.csv" -NoTypeInformation

# === EVENT LOGS ===

Write-Host "[6/8] Extracting critical event logs..."
$logQueries = @{
    "security-logons" = @{
        LogName = "Security"
        Id = @(4624, 4625, 4648, 4672, 4720, 4722, 4723, 4724, 4732, 4756)
    }
    "powershell" = @{
        LogName = "Microsoft-Windows-PowerShell/Operational"
        Id = @(4103, 4104)  # Script block logging
    }
    "sysmon" = @{
        LogName = "Microsoft-Windows-Sysmon/Operational"
        Id = @(1, 3, 7, 8, 10, 11, 13, 22, 23, 25)  # Process, network, image load, etc.
    }
}

foreach ($name in $logQueries.Keys) {
    $q = $logQueries[$name]
    try {
        Get-WinEvent -FilterHashtable @{
            LogName = $q.LogName; Id = $q.Id
            StartTime = (Get-Date).AddDays(-7)
        } -MaxEvents 10000 -ErrorAction Stop |
            Export-Csv "$outDir\events-$name.csv" -NoTypeInformation
    } catch {
        Write-Host "  [!] Could not collect $name logs: $_"
    }
}

# === FILE SYSTEM ARTIFACTS ===

Write-Host "[7/8] Collecting file system artifacts..."
# Recently modified executables and scripts
Get-ChildItem -Path C:\Users, C:\Windows\Temp, C:\ProgramData -Recurse `
    -Include *.exe, *.dll, *.ps1, *.bat, *.vbs, *.js -ErrorAction SilentlyContinue |
    Where-Object { $_.LastWriteTime -gt (Get-Date).AddDays(-30) } |
    Select-Object FullName, Length, CreationTime, LastWriteTime, LastAccessTime,
        @{N='SHA256';E={(Get-FileHash $_.FullName -Algorithm SHA256).Hash}} |
    Export-Csv "$outDir\recent-executables.csv" -NoTypeInformation

# Prefetch files (evidence of execution)
if (Test-Path "C:\Windows\Prefetch") {
    Get-ChildItem "C:\Windows\Prefetch\*.pf" |
        Select-Object Name, CreationTime, LastWriteTime |
        Export-Csv "$outDir\prefetch.csv" -NoTypeInformation
}

Write-Host "[8/8] Generating collection summary..."
$summary = @"
IR Triage Collection Summary
============================
System:     $env:COMPUTERNAME
Collected:  $(Get-Date -Format u) UTC
Analyst:    $env:USERNAME
Files:      $(Get-ChildItem $outDir | Measure-Object).Count artifacts
"@
$summary | Out-File "$outDir\COLLECTION-SUMMARY.txt"

Write-Host "[+] Triage complete: $outDir"
Write-Host "[!] NEXT: Image memory with WinPMEM or Magnet RAM Capture"
Write-Host "[!] NEXT: Copy $outDir to analysis workstation — do NOT analyze on compromised system"
```

### Linux Forensic Triage Script


A suspected host may already be hostile: use trusted collection tools and an
approved evidence destination. This local triage is not a substitute for a
forensic image. Restrict the collection directory to the collector, preserve it
for handoff, and never write through a pre-existing path supplied by another user.

```bash
#!/bin/bash
# Linux Incident Response Triage Collection
# Run as root on suspected compromised system

TIMESTAMP=$(date -u +"%Y%m%d-%H%M%S")
# Evidence can contain credentials. Create a private directory atomically;
# never reuse a predictable path in /tmp or trust an inherited TMPDIR.
umask 077
OUTDIR=$(mktemp -d /tmp/ir-triage.XXXXXXXXXX) || {
    echo "[!] Unable to create private evidence directory" >&2
    exit 1
}
readonly OUTDIR
# Preserve the directory for handoff; do not delete evidence in an EXIT trap.

echo "[*] Starting Linux IR triage at ${TIMESTAMP} UTC"

# === VOLATILE DATA ===
echo "[1/7] Capturing processes..."
ps auxwwf > "$OUTDIR/ps-tree.txt"
ls -la /proc/*/exe 2>/dev/null > "$OUTDIR/proc-exe-links.txt"
cat /proc/*/cmdline 2>/dev/null | tr '\0' ' ' > "$OUTDIR/proc-cmdline.txt"

echo "[2/7] Capturing network state..."
ss -tlnp > "$OUTDIR/listening-ports.txt"
ss -tnp > "$OUTDIR/established-connections.txt"
ip addr > "$OUTDIR/ip-addresses.txt"
ip route > "$OUTDIR/routing-table.txt"
iptables -L -n -v > "$OUTDIR/firewall-rules.txt" 2>/dev/null

echo "[3/7] Capturing user activity..."
w > "$OUTDIR/logged-in-users.txt"
last -50 > "$OUTDIR/last-logins.txt"
lastb -50 > "$OUTDIR/failed-logins.txt" 2>/dev/null

# === PERSISTENCE ===
echo "[4/7] Enumerating persistence mechanisms..."
# Cron jobs (all users)
for user in $(cut -f1 -d: /etc/passwd); do
    crontab -l -u "$user" 2>/dev/null | grep -v '^#' |
        sed "s/^/${user}: /" >> "$OUTDIR/crontabs.txt"
done
ls -la /etc/cron.* > "$OUTDIR/cron-dirs.txt" 2>/dev/null

# Systemd services (non-vendor)
systemctl list-unit-files --type=service --state=enabled |
    grep -v '/usr/lib/systemd' > "$OUTDIR/enabled-services.txt"

# SSH authorized keys
find /home /root -name "authorized_keys" -exec echo "=== {} ===" \; \
    -exec cat {} \; > "$OUTDIR/ssh-authorized-keys.txt" 2>/dev/null

# Shell profiles (backdoor injection point)
cat /etc/profile /etc/bash.bashrc /root/.bashrc /root/.bash_profile \
    > "$OUTDIR/shell-profiles.txt" 2>/dev/null

# === LOGS ===
echo "[5/7] Collecting log snippets..."
journalctl --since "7 days ago" -u sshd --no-pager > "$OUTDIR/sshd-logs.txt" 2>/dev/null
tail -10000 /var/log/auth.log > "$OUTDIR/auth-log.txt" 2>/dev/null
tail -10000 /var/log/secure > "$OUTDIR/secure-log.txt" 2>/dev/null
tail -5000 /var/log/syslog > "$OUTDIR/syslog.txt" 2>/dev/null

# === FILE SYSTEM ===
echo "[6/7] Finding suspicious files..."
# Recently modified files in sensitive directories
find /tmp /var/tmp /dev/shm /usr/local/bin /usr/local/sbin \
    -type f -mtime -30 -ls > "$OUTDIR/recent-suspicious-files.txt" 2>/dev/null

# SUID/SGID binaries (privilege escalation vectors)
find / -perm /6000 -type f -ls > "$OUTDIR/suid-sgid.txt" 2>/dev/null

# Files with no package owner (potential implants)
if command -v rpm &>/dev/null; then
    rpm -Va > "$OUTDIR/rpm-verify.txt" 2>/dev/null
elif command -v debsums &>/dev/null; then
    debsums -c > "$OUTDIR/debsums-changed.txt" 2>/dev/null
fi

echo "[7/7] Computing file hashes for key binaries..."
sha256sum /usr/bin/ssh /usr/sbin/sshd /bin/bash /usr/bin/sudo \
    /usr/bin/curl /usr/bin/wget > "$OUTDIR/critical-binary-hashes.txt" 2>/dev/null

echo "[+] Triage complete: $OUTDIR"
echo "[!] NEXT: Image memory with LiME or AVML"
echo "[!] NEXT: Copy to analysis workstation via SCP — verify SHA256 after transfer"
```

## SEV3 — Medium (Response: Next business day)

**Criteria**: Suspicious activity requiring investigation, policy violation
with potential security impact, vulnerability exploitation attempted
but blocked, phishing reported with no click.

| Action              | Timeline     | Owner        |
|---------------------|-------------|--------------|
| Analyst assignment  | 0-8 hours   | SOC Lead     |
| Initial analysis    | 0-24 hours  | SOC Analyst  |
| Resolution          | 0-72 hours  | IR Team      |

## SEV4 — Low (Response: Standard queue)

**Criteria**: Security policy violation (no compromise), informational
alerts from security tools, vulnerability scan findings, access
review discrepancies.

| Action              | Timeline     | Owner        |
|---------------------|-------------|--------------|
| Ticket creation     | 0-24 hours  | SOC          |
| Resolution          | 0-2 weeks   | Assigned team|
```

### Step 1: Detection & Triage (First 30 Minutes)

- Receive alert from SIEM, EDR, user report, or external notification (law enforcement, threat intel provider)
- Perform initial triage: is this a true positive? What is the scope? Is it active?
- Classify severity using the incident matrix and activate the appropriate response level
- Assemble the response team: IR lead, forensic analyst, IT operations, communications, legal (for SEV1-2)
- Open the incident ticket and begin the timeline — every action gets logged from this point

### Step 3: Investigation & Forensics (Hours to Days)

- Reconstruct the complete attack timeline: initial access, execution, persistence, lateral movement, exfiltration
- Identify all compromised systems, accounts, and data through log analysis, forensic imaging, and EDR telemetry
- Determine the root cause and all contributing factors — what failed, what was missing, what was ignored
- Collect and preserve evidence with forensic rigor — this may become a legal matter

### Cloud Incident Response

- AWS: CloudTrail log analysis, GuardDuty alert triage, IAM policy forensics, S3 access log investigation, Lambda invocation tracing
- Azure: Unified Audit Log analysis, Azure AD sign-in forensics, NSG flow log review, Defender for Cloud alert correlation
- GCP: Cloud Audit Logs, VPC Flow Logs, Security Command Center findings, service account key usage analysis
- Container forensics: pod inspection, image layer analysis, runtime behavior comparison against known-good baselines

### Threat Intelligence Integration

- Correlate IOCs against threat intelligence platforms (MISP, OTX, VirusTotal) to identify threat actor and campaign
- Map observed TTPs to MITRE ATT&CK for structured analysis and detection gap identification
- Produce actionable threat intelligence from incident findings — share IOCs and detection rules with ISACs and trusted peers
- Use YARA rules for retroactive hunting across the environment — find the same malware family on other systems

### Crisis Communication

- Draft breach notification letters that meet GDPR (72 hours), state breach notification laws, and sector-specific requirements (HIPAA, PCI-DSS)
- Coordinate with external parties: law enforcement, regulators, cyber insurance carriers, third-party forensic firms
- Manage media inquiries with prepared statements that are accurate without providing attacker intelligence
- Run tabletop exercises that simulate realistic incidents and test organizational response procedures

---

**Instructions Reference**: Your methodology aligns with NIST SP 800-61 (Computer Security Incident Handling Guide), SANS Incident Response Process, FIRST CSIRT framework, and the hard-won lessons from thousands of real-world incidents.

