# Architecture

Dokumen ini menjelaskan komponen `shorts-workflow-skill` dan keputusan
desain di baliknya.

---

## Overview

`shorts-workflow-skill` adalah **agent skill** — bukan aplikasi standalone,
bukan service, bukan FFmpeg pipeline. Skill ini di-load oleh agent /
orchestrator dan berisi instruksi terstruktur untuk mengubah SRT/transcript
menjadi rencana produksi short recap-commentary.

```
┌────────────────────────────────────────────────────────┐
│              AGENT / ORCHESTRATOR                       │
│        (MoClaw, OpenClaw, atau LLM apa pun)             │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │      shorts-workflow skill                       │   │
│  │      (SKILL.md = instruksi A–J)                  │   │
│  │                                                  │   │
│  │   templates/   docs/   examples/                 │   │
│  └─────────────────────────────────────────────────┘   │
└────────────────────────────────────────────────────────┘
              ▲                         │
              │ SRT / transcript        │ Markdown plan
              │                         ▼
        ┌─────────┐               ┌──────────────┐
        │  User   │               │  CapCut      │
        └─────────┘               │  (manual)    │
                                  └──────────────┘
```

Skill ini **TIDAK**:

- Memproses video atau audio.
- Menjalankan FFmpeg.
- Memanggil Gemini / OpenAI / API video pihak ketiga.
- Mengupload apa pun ke YouTube / TikTok.
- Menjalankan MCP server / team mode / multi-agent worktree.

Skill ini **HANYA**:

- Memberi instruksi terstruktur ke LLM agent.
- Menyediakan templates Markdown yang konsisten.
- Mendefinisikan pipeline 10 step + scoring rubric.
- Menyediakan referensi (docs + examples).

---

## Komponen

### 1. SKILL.md — Skill Definition

Entry point. Berisi:

- YAML frontmatter (`name`, `description`) untuk discovery.
- Identitas agent (Recap Commentary Editor).
- Default values (durasi, aspect ratio, audience, bahasa).
- Daftar kategori scene universal.
- Framework storytelling (Apex Drop → Platform Sensitivity).
- Pipeline 10 step.
- Aturan non-negosiasi.
- Error handling.

### 2. Templates (`templates/*.md`)

Delapan template, satu per output bagian:

| File | Output |
|---|---|
| `scene-candidates.md` | A. Scene Candidates |
| `best-scene.md` | B. Best Scene |
| `story-arc.md` | C. Story Arc |
| `vo-script.md` | D. VO Narration Script |
| `visual-beat-map.md` | E. Visual Beat Map |
| `keep-cut-gap.md` | F. KEEP/CUT/GAP Plan |
| `packaging.md` | I. Packaging |
| `capcut-checklist.md` | J. CapCut Editing Checklist |

Template untuk output **G. Subtitle Plan** dan **H. Retention Review**
embed langsung di SKILL.md karena strukturnya sederhana.

Templates **bukan executable code**. Mereka adalah referensi struktur.
Agent membaca template lalu menghasilkan output dalam format yang sama.

### 3. Examples (`examples/`)

- `sample-input.srt` — SRT fiksi/generic, bukan show nyata.
- `sample-output.md` — full A–J berdasarkan sample-input.srt.

Berfungsi sebagai **few-shot reference** untuk agent.

### 4. Docs (`docs/`)

- `recap-commentary-playbook.md` — filosofi, gaya VO, do/don't.
- `architecture.md` — file ini.
- `workflow.md` — detail eksekusi per step + edge cases.
- `scoring-rubric.md` — rubric 100 poin + grade boundary.
- `platform-sensitivity.md` — checklist konten sensitif.
- `omo-adaptation-notes.md` — pola yang diadaptasi & tidak.

---

## Design Decisions

### D1. Text-only di v0.1

**Keputusan:** Tidak ada FFmpeg, Gemini Vision, atau upload otomatis.

**Alasan:**

- Skill bisa dipakai LLM mana pun tanpa tool dependency.
- Manual editing di CapCut sudah jadi default workflow user.
- Menambah video pipeline berarti menambah surface error yang besar
  (codec, GPU, rate limit, billing) yang tidak relevan untuk versi awal.
- Ditambahkan nanti sebagai **layer terpisah** kalau benar-benar perlu.

### D2. Single SKILL.md, bukan multi-skill

**Keputusan:** Satu file SKILL.md yang load semua instruksi. Bukan
sub-skill `scene-scorer`, `vo-writer`, `packaging-bot`, dst.

**Alasan:**

- Domain ini cukup sempit (1 jenis konten, 1 platform family).
- Single file lebih portable — copy-paste ke system prompt langsung jalan.
- Orchestrator yang mau memecah ke sub-agent bisa pakai parallelism hint
  di pipeline tanpa butuh skill terpisah.

### D3. Templates sebagai Markdown, bukan JSON schema

**Keputusan:** Format output adalah Markdown, dengan tabel-tabel
sederhana.

**Alasan:**

- LLM lebih natural menulis Markdown dibanding JSON.
- Output bisa langsung dibaca user / dipindahkan ke Notion / CapCut notes.
- Tidak perlu validator atau parser.

### D4. Scoring Rubric Eksplisit

**Keputusan:** Rubric 100 poin dengan 9 kriteria positif + 1 penalti.

**Alasan:**

- Tanpa rubric, LLM cenderung "everything is 7/10".
- Bobot mencerminkan realita short-form: hook (20) + standalone (15) +
  stakes (15) > everything else.
- Penalti `Platform Risk` -10..0 memaksa agent peduli pada policy.
- Lihat [`scoring-rubric.md`](scoring-rubric.md).

### D5. Bahasa Bilingual

**Keputusan:** Dokumentasi & reasoning Indonesia, output (VO, title,
caption, hook, subtitle) English.

**Alasan:**

- User berbahasa Indonesia, lebih cepat membaca panduan dalam bahasa ibu.
- Audience target US, jadi output harus English natural.
- Istilah teknis (hook, retention, CTA, B-roll, beat) dipertahankan
  English karena tidak ada padanan natural.

### D6. General-Purpose, Bukan Show-Specific

**Keputusan:** Skill tidak hardcode ke Chicago Fire, firefighter, rescue,
medical, atau show tertentu.

**Alasan:**

- User sudah mengonfirmasi recap commentary umum bisa diadaptasi ke
  banyak show (medical drama, crime, sci-fi, family drama).
- Hardcoding domain memaksa rewrite skill setiap kali pivot.
- Daftar kategori scene universal cukup luas untuk menangani genre umum,
  dan agent bisa memberi label `other:` jika perlu.

### D7. Vertical 9:16, Durasi 80–110s

**Keputusan:** Default ke 9:16 dan 80–110 detik.

**Alasan:**

- Format ini "sweet spot" YouTube Shorts + TikTok + Reels secara
  bersamaan.
- 80–110s memberi ruang untuk Apex Drop + setup + escalation + twist +
  payoff. Lebih pendek (≤45s) tidak cukup. Lebih panjang (>110s)
  menurunkan completion rate.
- User bisa override.

### D8. Manual CapCut, Bukan Auto-Cut

**Keputusan:** Output adalah panduan editing manual, bukan EDL atau
auto-cut script.

**Alasan:**

- Editor manusia masih punya intuisi yang sulit ditiru: kapan freeze,
  kapan slow-mo, kapan jump-cut.
- Skill text-based tetap berguna walaupun editor pakai Premiere /
  DaVinci / Final Cut, bukan CapCut.
- Auto-cut bisa ditambahkan di v0.3+ sebagai layer opsional.

---

## Integrasi

### Agent Orchestrator (platform-neutral)

```
Load shorts-workflow skill, then provide SRT/transcript as input.
[paste .srt + opsional: target durasi, judul show, override default]
```

Skill ini load-once-and-execute — orchestrator yang men-trigger pipeline
Step 1–10. Tidak ada dependency ke kategori task tertentu di host
platform.

### Standalone (LLM apa pun)

```
System prompt: [paste isi SKILL.md]
User: Buatkan recap commentary short dari .srt berikut. [paste .srt]
```

### Multi-Agent (opsional, future)

Pipeline mengandung parallelism hint:

```
Wave 1: Step 1 (parse) + Step 2 (scene candidates)        [sequential]
Wave 2: Step 3 (best scene) + Step 4 (story arc)          [sequential]
Wave 3: Step 5 (VO) || Step 6 (visual beat map)           [parallel]
Wave 4: Step 7 (KEEP/CUT/GAP) + Step 8 (subtitle)         [sequential]
Wave 5: Step 9 (retention review)                         [sequential]
Wave 6: Step 10 (packaging) || Step 10 (CapCut checklist) [parallel]
```

Tidak wajib dipecah. Single agent yang menjalankan berurutan juga sah.

---

## File Map

```
shorts-workflow-skill/
├── SKILL.md
├── README.md
├── examples/
│   ├── sample-input.srt
│   └── sample-output.md
├── templates/
│   ├── scene-candidates.md
│   ├── best-scene.md
│   ├── story-arc.md
│   ├── vo-script.md
│   ├── visual-beat-map.md
│   ├── keep-cut-gap.md
│   ├── packaging.md
│   └── capcut-checklist.md
└── docs/
    ├── recap-commentary-playbook.md
    ├── architecture.md
    ├── workflow.md
    ├── scoring-rubric.md
    ├── platform-sensitivity.md
    └── omo-adaptation-notes.md
```
