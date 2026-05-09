# Architecture

Dokumen ini menjelaskan arsitektur shorts-workflow-skill dan design decisions di baliknya.

---

## Overview

shorts-workflow-skill adalah **agent skill** — bukan standalone application. Skill ini di-load oleh agent orchestrator (MoClaw, OpenClaw, atau sejenisnya) dan memberikan instruksi terstruktur untuk mengubah transcript/subtitle menjadi editing plan konten short-form.

```
┌─────────────────────────────────────────────┐
│           AGENT ORCHESTRATOR                 │
│        (MoClaw / OpenClaw / etc.)            │
│                                              │
│  ┌─────────────────────────────────────┐     │
│  │     shorts-workflow skill           │     │
│  │     (SKILL.md = instruksi)          │     │
│  └─────────────────────────────────────┘     │
│                                              │
│  ┌──────┐  ┌──────┐  ┌──────┐  ┌──────┐    │
│  │Agent │  │Agent │  │Agent │  │Agent │     │
│  │  1   │  │  2   │  │  3   │  │  N   │     │
│  └──────┘  └──────┘  └──────┘  └──────┘     │
└─────────────────────────────────────────────┘
```

Skill ini TIDAK:
- Memproses video atau audio
- Memanggil API eksternal
- Menjalankan FFmpeg atau tool editing
- Memerlukan database atau storage

Skill ini HANYA:
- Memberikan instruksi terstruktur ke LLM agent
- Menyediakan template untuk output yang konsisten
- Mendefinisikan pipeline 10-step yang bisa dieksekusi secara sequential atau partial

---

## Komponen

### 1. SKILL.md — Skill Definition

File utama yang di-load oleh orchestrator. Berisi:
- YAML frontmatter (`name`, `description`) untuk discovery
- Instruksi lengkap 10-step pipeline
- Default values (audience, platform, duration, dll)
- Error handling rules
- Parallelism hints untuk orchestrator

### 2. Templates

Empat template Markdown yang menjaga konsistensi output:

| Template | Fungsi | Digunakan di Step |
|----------|--------|-------------------|
| `scene-candidate-table.md` | Scoring framework per segment | Step 2 |
| `cut-plan.md` | Format tabel cut dengan labels | Step 4-5 |
| `vo-script.md` | Struktur voiceover script | Step 6 |
| `packaging.md` | Title/caption/hashtag + CapCut checklist | Step 9-10 |

Templates bersifat **referensi** — agent membaca template lalu menghasilkan output dalam format yang sama. Templates BUKAN executable code.

### 3. Examples

Contoh input-output konkret:
- `sample-input.srt`: File .srt realistis (~2:46 video tentang AI coding tools)
- `sample-output.md`: Output lengkap dari full pipeline berdasarkan sample input

Examples berfungsi sebagai few-shot reference untuk agent.

### 4. Docs

Dokumentasi arsitektur dan workflow:
- `architecture.md` (file ini)
- `workflow.md`: Detail per-step dengan edge cases
- `omo-adaptation-notes.md`: Pola yang diadaptasi dari Oh-My-OpenAgent

---

## Design Decisions

### Text-Only (v0.1)

**Keputusan**: Tidak ada video processing, FFmpeg, atau Gemini Vision.

**Alasan**: 
- Mengurangi complexity secara drastis
- Skill bisa digunakan oleh ANY LLM agent tanpa tool dependencies
- Transcript/subtitle sudah mengandung informasi yang cukup untuk editing plan
- Video processing bisa ditambahkan di v0.2+ sebagai layer terpisah

### Single SKILL.md (bukan multi-agent)

**Keputusan**: Satu skill file, bukan 10 skill files terpisah per step.

**Alasan**:
- Lebih portable — load 1 file, dapat semua capability
- Orchestrator yang mendelegasikan ke subagent bisa memecah pipeline sendiri
- Menghindari over-engineering di fase awal
- Jika nanti perlu multi-agent, SKILL.md sudah menyertakan parallelism hints

### Template sebagai Markdown (bukan JSON schema)

**Keputusan**: Templates dalam format Markdown table, bukan JSON/YAML schema.

**Alasan**:
- LLM lebih natural menghasilkan Markdown tables
- Markdown readable oleh manusia tanpa parser
- Tidak butuh validation layer — agent mengikuti format by instruction
- Compatible dengan semua platform (GitHub, Notion, CapCut notes, dll)

### Scoring Formula yang Explicit

**Keputusan**: Weighted scoring (Hook 25%, Emotion 25%, Info 20%, Visual 15%, Standalone 15%) di-hardcode dalam skill.

**Alasan**:
- Tanpa formula explicit, LLM cenderung "everything is 7/10"
- Weights bisa di-override user via prompt, tapi default harus opinionated
- Bobot mencerminkan realita short-form: hook dan emotion > everything else

### Bahasa Indonesia + English Terms

**Keputusan**: Penjelasan dalam bahasa Indonesia, istilah teknis dan output (title/caption/hashtag) dalam bahasa Inggris.

**Alasan**:
- User berbahasa Indonesia
- Target audience US — metadata harus English
- Istilah teknis (hook, retention, CTA, B-roll) tidak punya padanan Indonesia yang natural

---

## Integrasi dengan Orchestrator

### MoClaw / OpenClaw

```
task(
  category="creative-writing",
  load_skills=["shorts-workflow"],
  prompt="[user request + transcript]"
)
```

Orchestrator akan:
1. Load SKILL.md ke system prompt agent
2. Agent membaca instruksi dan templates
3. Agent mengeksekusi pipeline berdasarkan user request
4. Output: Markdown document sesuai format

### Multi-Agent (Future)

Jika orchestrator mendukung delegation:

```
Wave 1 (parallel):
  task(category="analysis", load_skills=["shorts-workflow"], prompt="Step 2: Scene analysis for [transcript]")
  task(category="creative", load_skills=["shorts-workflow"], prompt="Step 6: Generate hook options for [topic]")

Wave 2 (after Wave 1):
  task(category="creative", load_skills=["shorts-workflow"], prompt="Step 4-5: Cut plan based on [Wave 1 results]")

Wave 3 (after Wave 2):
  task(category="creative", load_skills=["shorts-workflow"], prompt="Step 8: Retention review for [full plan]")

Wave 4 (after Wave 3):
  task(category="metadata", load_skills=["shorts-workflow"], prompt="Step 9-10: Metadata + CapCut checklist")
```

### Standalone (tanpa orchestrator)

Copy isi SKILL.md ke system prompt LLM mana pun:

```
System: [paste SKILL.md content]
User: Analisis .srt ini dan buat editing plan: [paste .srt]
```

---

## File Map

```
shorts-workflow-skill/
├── SKILL.md                          # LOAD INI — skill definition
├── README.md                         # Overview & usage
├── examples/
│   ├── sample-input.srt              # Contoh input
│   └── sample-output.md              # Contoh output
├── templates/
│   ├── scene-candidate-table.md      # Scoring template
│   ├── cut-plan.md                   # Cut plan template
│   ├── vo-script.md                  # VO script template
│   └── packaging.md                  # Metadata + CapCut checklist
└── docs/
    ├── architecture.md               # File ini
    ├── workflow.md                    # Detail per-step
    └── omo-adaptation-notes.md       # Pola dari OMO
```
