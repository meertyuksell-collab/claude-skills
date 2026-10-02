---
name: nexus-orchestration
description: Use when coordinating multiple specialist agents/skills on a large multi-phase project (discovery → strategy → build → launch → operate). NEXUS is a phased orchestration playbook with handoff templates and scenario runbooks (MVP, enterprise feature, marketing campaign, incident response).
license: MIT
---

# NEXUS — Çok-ajanlı orkestrasyon

> Kaynak: [The Agency / NEXUS](https://github.com/msitarzewski/agency-agents) (MIT, © AgentLand Contributors).

Büyük işleri fazlara bölüp birden çok uzman skill'i sırayla/paralel çalıştırmak için bir protokol. Tek skill'lik işler için gerek yok; bunun yerine ilgili uzman skill'i doğrudan kullan.

## Ne zaman

- İş birden çok alanı ve aşamayı kapsıyorsa (ürün keşfi + mimari + geliştirme + lansman).
- Birden çok skill arasında düzenli devir-teslim (handoff) gerekiyorsa.

## Nasıl kullanılır

1. **Fazı seç:** `references/phase-0-discovery.md` … `phase-6-operate.md` — her biri o fazın amacını, gird/çıktılarını ve hangi uzmanların devrede olduğunu anlatır.
2. **Senaryo runbook'u:** Hazır akışlar `references/scenario-*.md` (MVP, kurumsal özellik, pazarlama kampanyası, olay müdahalesi).
3. **Devir-teslim:** `references/handoff-templates.md` ve `references/agent-activation-prompts.md` ajanlar arası standart aktarım biçimini verir.
4. **Tam protokol:** Ayrıntı için `references/nexus-strategy.md`.

Her fazda ilgili uzman skill'i kütüphaneden çağır (bkz. yönlendirici).
