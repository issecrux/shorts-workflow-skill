# shorts-workflow-skill

Agent Skill portable untuk workflow pembuatan konten **YouTube Shorts / TikTok / Reels** dari subtitle (.srt) atau transcript.

Versi text-based — tidak memerlukan FFmpeg, Gemini Vision, atau video upload.

## Apa Ini?

Skill file (`SKILL.md`) yang bisa di-load oleh agent orchestrator (MoClaw, OpenClaw, atau tool serupa) untuk mengubah transcript/subtitle menjadi editing plan lengkap:

1. Analisis scene + engagement scoring
2. Pemilihan scene terbaik
3. Timestamp cut plan (KEEP/CUT/GAP)
4. Voiceover script
5. Dialog preservation map
6. Retention review
7. Title, caption, hashtag
8. CapCut editing checklist

## Quick Start

### Untuk MoClaw / OpenClaw

Load skill saat delegasi:

```
task(
  category="creative-writing",
  load_skills=["shorts-workflow"],
  prompt="Analisis file .srt ini dan buat full editing plan untuk TikTok: [paste .srt content]"
)
```

### Untuk Manual Use

Copy isi `SKILL.md` ke system prompt agent, lalu berikan transcript sebagai user input.

## Struktur

```
shorts-workflow-skill/
  SKILL.md                          # Skill definition — load ini
  README.md                         # File ini
  examples/
    sample-input.srt                # Contoh input subtitle
    sample-output.md                # Contoh output lengkap
  templates/
    scene-candidate-table.md        # Template tabel analisis scene
    cut-plan.md                     # Template cut plan
    vo-script.md                    # Template VO script
    packaging.md                    # Template title/caption/hashtag + CapCut checklist
  docs/
    architecture.md                 # Arsitektur dan design decisions
    workflow.md                     # Detail alur kerja per step
    omo-adaptation-notes.md         # Catatan adaptasi dari Oh-My-OpenAgent
```

## Default

| Setting | Value |
|---------|-------|
| Target audience | US, 18-34 |
| Platform | YouTube Shorts, TikTok, Reels |
| Max duration | 60 detik |
| Hook window | 3 detik pertama |
| Gaya editing | Cepat, tight cut, minim dead air |
| Bahasa penjelasan | Indonesia |
| Title/caption/hashtag | English (untuk US audience) |

## Contoh Penggunaan

**Full pipeline:**
```
Analisis .srt ini dan buat editing plan lengkap untuk YouTube Shorts:

[paste .srt content]
```

**Partial — hanya VO script:**
```
Buat VO script untuk scene berikut, target 45 detik, TikTok style:

[paste selected scene transcript]
```

**Partial — hanya metadata:**
```
Buat title, caption, dan hashtag untuk short tentang:
Topic: [topic]
Hook: [hook summary]
Platform: TikTok
```

## Roadmap

- [x] v0.1 — Text-based pipeline (current)
- [ ] v0.2 — Multi-language support (ID, EN, mixed)
- [ ] v0.3 — Integration dengan FFmpeg untuk auto-cut
- [ ] v0.4 — Gemini Vision untuk scene analysis dari video
- [ ] v0.5 — Batch processing (multiple shorts dari satu video)

## Adaptasi dari OMO

Arsitektur skill ini diadaptasi dari pola orchestration [Oh-My-OpenAgent](https://github.com/code-yeongyu/oh-my-openagent). Detail adaptasi ada di `docs/omo-adaptation-notes.md`.

## Lisensi

MIT
