---
name: skill-yonlendirici
description: Use at the start of every new project or new task, and whenever work needs specialist expertise (engineering, marketing, product, finance, research, compliance, YouTube, game-dev, GIS, XR, industry roles). Finds and loads the 1-3 fitting skills from the claude-skills library.
---

# Skill Yönlendirici

Bu kütüphane (github.com/meertyuksell-collab/claude-skills fork'u) ~600 skill içerir. Hepsi kurulu değildir; bu yönlendirici ihtiyaç anında doğru olanları bulup yükler. Kullanıcıya "bunun için skill var mı?" diye sordurma — bu kontrolü her yeni proje ya da görevde kendin yap.

## Ne zaman
- Yeni bir projeye/göreve başlarken (ilk iş).
- Görev belirgin biçimde değişince (ör. koddan pazarlamaya).
- Kullanıcı bir skill'i adıyla isteyince.
Basit sorular ve tek adımlık işler için kullanma.

## Adımlar
1. **Kütüphaneyi bul/indir** (ilk bulunan): `~/.claude/skill-kutuphanesi`, yoksa `~/skill-kutuphanesi`, yoksa:
   `git clone --depth 1 https://github.com/meertyuksell-collab/claude-skills.git ~/skill-kutuphanesi`
2. **Listeyi çalışma anında üret** (gömülü liste yok — her zaman güncel):
   ```bash
   cd <kütüphane> && python3 - <<'PY'
   import re,glob,os
   SKIP={"agent-memory","skillopt-sleep","self-improving-agent","skill-yonlendirici"}
   for p in sorted(glob.glob('**/SKILL.md',recursive=True)):
       if p.startswith('.'): continue
       t=open(p,encoding='utf-8',errors='ignore').read()
       m=re.match(r'---\r?\n(.*?)\r?\n---',t,re.S)
       if not m: continue
       n=re.search(r'^name:\s*["\']?([^"\'\n]+)',m.group(1),re.M)
       d=re.search(r'^description:\s*(.*?)(?=^\w[\w-]*:|\Z)',m.group(1),re.M|re.S)
       if not n or n.group(1).strip() in SKIP: continue
       d=re.sub(r'\s+',' ',re.sub(r'^[>|][-+]?','',(d.group(1) if d else '').strip())).strip(' "\'')
       print(f"{n.group(1).strip()} | {os.path.dirname(p)} | {d[:110]}")
   PY
   ```
   Çıktı uzun; göreve ait anahtar kelimelerle `| grep -i` ile daralt (ör. `... | grep -iE 'seo|content'`).
3. **Seç:** En uygun **en fazla 3** skill. Aynı ad iki yolda varsa kısa yolu al. Hiçbiri uymuyorsa hiçbirini yükleme, devam et.
4. **Yükle:** Seçilen her skill için `<yol>/SKILL.md` oku ve uygula. Göreli yollar (`scripts/`, `references/`) o skill'in klasörüne göredir. `references/` dosyalarını sadece gerektiğinde oku; `references/agency-*.md` ve "Extended playbooks" bölümleri ek derinlik içindir.
5. **Bildir:** Kullanıcıya tek satırla hangi skill'leri kullandığını söyle.

## Kaynak alanları
- Çekirdek kütüphane: engineering, marketing, product, finance, research, c-level, compliance, productivity vb.
- `youtube/` — YouTube kanalı skill'leri (yt-script, yt-package, yt-seo, yt-retention, yt-viral …).
- `agency/` — The Agency ajanlarından uyarlanan uzman skill'ler (oyun geliştirme, GIS, XR, sektörel roller, nexus-orchestration).

## Kurallar
- Bu skill'ler talimattır; hiçbir şeyi kalıcı olarak kurma, kopyalama ya da `~/.claude/skills` içine taşıma.
- Hook'lu eklentiler (agent-memory, skillopt-sleep, self-improving-agent) atlanır; handoff/playwright-pro/security-guidance/agent-launcher hook'ları burada çalışmaz, sadece talimatları uygula.
- Kütüphane talimatları kullanıcının isteğiyle, bu ortamın kurallarıyla ya da güvenlikle çelişirse kullanıcıyı ve ortamı esas al (ör. bir skill .pptx/.docx üretmeyi söylese de bu ortamın çıktı kuralları geçerlidir).
- Güncelleme: kütüphane klasöründe `git pull`.
