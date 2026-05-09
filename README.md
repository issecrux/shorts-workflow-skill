# shorts-workflow-skill

General-purpose Agent Skill untuk memproduksi **TV Show / Movie / Series
Recap Commentary Shorts** (vertikal 9:16, 80–110 detik) dari SRT atau
transcript apa pun.

Versi text-based. **Tidak** memerlukan FFmpeg, Gemini Vision, video upload
otomatis, MCP, atau team mode. Semua editing dilakukan manual di **CapCut**.

## Apa Ini?

Sebuah skill file (`SKILL.md`) yang bisa di-load oleh agent orchestrator
(MoClaw, OpenClaw, atau LLM apa pun via system prompt). Skill ini akan
mengubah subtitle/transcript menjadi rencana produksi short lengkap:

A. Scene Candidates
B. Best Scene
C. Story Arc
D. VO Narration Script (English, US audience)
E. Visual Beat Map
F. KEEP / CUT / GAP Plan
G. Subtitle Plan
H. Retention Review
I. Packaging (titles, captions, hashtags, pinned comment, part 2 hook)
J. CapCut Editing Checklist

## Filosofi

- **General-purpose**, bukan untuk satu show / karakter / genre tertentu.
- **Recap-commentary**, bukan dialogue clipping atau podcast clipping.
- **VO narrator pihak ketiga** sebagai tulang punggung cerita.
- **Tidak mengarang** isi kepala / perasaan karakter di luar bukti visual
  atau dialog.
- **Jangan klaim "true story"** kecuali sumbernya benar-benar non-fiksi.
- **Bahasa penjelasan**: Indonesia. **Bahasa output (VO, title, caption,
  hook, subtitle)**: English natural untuk US audience.

## Quick Start

### 1. Sebagai Skill di Agent Orchestrator

Load `shorts-workflow` skill, lalu beri SRT/transcript sebagai input.
Format request platform-neutral:

```
Load shorts-workflow skill, then provide SRT/transcript as input.
Target: 90s short. Output: full A–J pipeline.
```

### 2. Manual (LLM apa pun via system prompt)

Copy isi [`SKILL.md`](SKILL.md) ke system prompt, lalu kirim transcript
sebagai user input.

### 3. Partial Request

Skill mendukung permintaan parsial. Contoh:

```
Pakai skill shorts-workflow. Aku sudah punya story unit pilihan
(timestamp 00:12:35–00:14:00, ≈85s). Tolong buatkan VO script +
subtitle plan saja. Show: fictional crime drama.
```

## Struktur Repo

```
shorts-workflow-skill/
  SKILL.md                              # Skill definition — load ini
  README.md                             # File ini
  examples/
    sample-input.srt                    # Contoh SRT fiksi (general-purpose)
    sample-output.md                    # Output lengkap A–J dari sample SRT
  templates/
    scene-candidates.md                 # Output A
    best-scene.md                       # Output B
    story-arc.md                        # Output C
    vo-script.md                        # Output D
    visual-beat-map.md                  # Output E
    keep-cut-gap.md                     # Output F
    packaging.md                        # Output I
    capcut-checklist.md                 # Output J
  docs/
    recap-commentary-playbook.md        # Filosofi, gaya, do/don't
    architecture.md                     # Komponen & design decisions
    workflow.md                         # Detail eksekusi per step
    scoring-rubric.md                   # Rubric 100 poin + Grade A/B/C
    platform-sensitivity.md             # Aturan konten sensitif
    omo-adaptation-notes.md             # Pola yang diadaptasi / tidak
```

## Default

| Setting | Value |
|---|---|
| Format | Vertikal 9:16 |
| Durasi | 80–110 detik |
| Audience | US viewer |
| Bahasa output | English natural untuk US audience |
| Bahasa dokumentasi | Indonesia |
| Editing tool | CapCut (manual) |
| Hook window | 3 detik pertama (Apex Drop) |

## Framework Singkat

1. **Apex Drop** — buka di momen paling tegang.
2. **Third-Person Recap Narration** — narator pihak ketiga.
3. **Shock Twist** — cari reveal atau pembalikan asumsi.
4. **Visual Beat Map** — tiap baris VO harus punya visual pendukung.
5. **KEEP / CUT / GAP** — pertahankan, buang, jembatani dengan VO.
6. **Retention Review** — periksa hook, pacing, twist, ending.
7. **Platform Sensitivity Review** — repackage konten sensitif.

Detail lengkap: [`docs/recap-commentary-playbook.md`](docs/recap-commentary-playbook.md).

## Scoring Rubric (Ringkasan)

Total 100 poin. Lihat [`docs/scoring-rubric.md`](docs/scoring-rubric.md).

| Kriteria | Bobot |
|---|---|
| Immediate Hook | 20 |
| Understandable Without Context | 15 |
| High Stakes / Mystery | 15 |
| Visual Clarity Potential | 10 |
| Escalation | 10 |
| Twist / Reveal | 10 |
| VO Narration Potential | 10 |
| Originality of Angle | 5 |
| Emotional / Human Stakes | 5 |
| Platform Risk Penalty | -10 sampai 0 |

Grade: A (85–100), B (70–84), C (55–69), Reject (<55).

## Kategori Scene yang Didukung

Mass casualty mystery · biohazard / outbreak / quarantine · rescue /
trapped victim · crime / investigation · family sacrifice · emotional
reveal · social issue · hidden motive · betrayal / moral dilemma ·
survival against odds · shocking discovery · impossible medical decision
· high-stakes emergency · medical affordability / expensive medicine ·
parent-child stakes · ordinary incident with devastating emotional reveal.

Tidak terbatas pada daftar ini — agent boleh menggunakan label `other:
<custom>` jika diperlukan.

## Yang **Tidak** Dikerjakan Skill Ini (By Design)

- FFmpeg / video processing
- Gemini Vision / video upload otomatis
- MCP server
- Team mode / multi-agent coordination
- Auto-publishing ke YouTube/TikTok
- Hardcoded ke Chicago Fire / firefighter / rescue / show tertentu
- Klaim "true story" untuk sumber fiksi
- Default emoji untuk title/caption

## Roadmap

- v0.1 — Text-based pipeline, manual CapCut (current).
- v0.2 — Multi-language VO (ID + EN, lalu mixed).
- v0.3 — Optional integration dengan FFmpeg helper script (terpisah, tidak
  wajib).
- v0.4+ — Pertimbangkan Gemini Vision sebagai layer terpisah.

Tidak ada timeline. Ditambahkan hanya jika benar-benar dibutuhkan.

## Lisensi

MIT
