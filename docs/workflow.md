# Workflow Detail

Dokumen ini menjelaskan detail tiap step dalam pipeline shorts-workflow, termasuk edge cases dan tips.

---

## Pipeline Overview

```
Input (.srt / transcript)
  │
  ├── Step 1: Parse Input
  ├── Step 2: Analisis Scene (scoring)
  ├── Step 3: Pilih Scene Terbaik
  ├── Step 4: Timestamp Cut Plan
  ├── Step 5: Labeling KEEP/CUT/GAP
  ├── Step 6: VO Script
  ├── Step 7: Dialog Preservation Map
  ├── Step 8: Retention Review
  ├── Step 9: Title, Caption, Hashtag
  └── Step 10: CapCut Editing Checklist
  │
  ▼
Output (Markdown document)
```

### Dependency Graph

```
Step 1 ──→ Step 2 ──→ Step 3 ──→ Step 4 ──→ Step 5
                                      │          │
                                      ▼          ▼
                                   Step 6    Step 7
                                      │          │
                                      └────┬─────┘
                                           ▼
                                        Step 8
                                           │
                                      ┌────┴────┐
                                      ▼         ▼
                                   Step 9   Step 10
```

### Parallelism Opportunities

| Pair | Bisa Paralel? | Alasan |
|------|---------------|--------|
| Step 2 + Step 6 | Tidak | Step 6 butuh scene selection (Step 3) |
| Step 6 + Step 7 | Ya | VO dan dialog preservation independent |
| Step 9 + Step 10 | Ya | Metadata dan CapCut checklist independent |
| Step 1 + Step 2 | Tidak | Step 2 butuh parsed input |

---

## Step 1: Parse Input

### .srt Format

Standard SubRip format:
```
[sequence number]
[start] --> [end]
[text]
[blank line]
```

**Parsing rules:**
- Sequence number: ignore (renumber sequentially)
- Timestamp format: `HH:MM:SS,mmm` — konversi koma ke titik untuk konsistensi
- Multi-line text: gabungkan dengan spasi
- HTML tags dalam text (italic, bold): strip untuk analisis, note untuk VO

### Plain Text

Jika input tidak mengandung timestamp:
- Segmentasi per kalimat (split by `. `, `? `, `! `)
- Estimasi durasi: ~150 kata/menit (2.5 kata/detik)
- Assign timestamp berdasarkan estimasi kumulatif

### Edge Cases

| Case | Handling |
|------|----------|
| .srt dengan timing overlap | Gunakan start time segment berikutnya sebagai end time segment sebelumnya |
| Subtitle kosong (hanya timestamp) | Skip — tandai sebagai silence/pause |
| Durasi segment > 30 detik | Split secara natural (per kalimat) |
| Non-English transcript | Proses normal — scoring language-agnostic |
| Mixed language (code-switch) | Tandai di notes — berguna untuk authenticity |

---

## Step 2: Analisis Scene

### Segmentation

Grup subtitle blocks per ~15 detik (atau per natural break):
- Prefer break di akhir kalimat
- Jangan break di tengah gagasan
- Minimum segment: 5 detik
- Maximum segment: 20 detik

### Scoring Guide

**Hook Potential (25%)**
- 1-3: Informatif tapi datar, tidak ada element curiosity
- 4-6: Ada element menarik tapi bisa di-skip
- 7-8: Pertanyaan, kontra-intuisi, atau surprise yang memicu "tell me more"
- 9-10: Impossible to skip — statement yang membuat viewer HARUS tahu jawabannya

**Emotional Peak (25%)**
- 1-3: Monoton, factual delivery
- 4-6: Ada variasi tone, humor ringan, atau mild tension
- 7-8: Clear emotional moment — tawa, kekagetan, frustasi, revelation
- 9-10: Viral-worthy reaction — extreme contrast, jaw-drop moment

**Information Density (20%)**
- 1-3: Mostly filler, "um", repetisi, atau transisi
- 4-6: Mix content dan filler, bisa dikondensasi
- 7-8: Setiap kalimat memberi value baru
- 9-10: Setiap KATA essential — hapus satu kata dan meaning berubah

**Visual Potential (15%)**
- 1-3: Pure talking head tanpa gesture
- 4-6: Ada gesture, ekspresi, atau screen share
- 7-8: Action, demonstration, before/after, transformation visible
- 9-10: Inherently visual — tidak butuh narasi untuk impactful

**Standalone Clarity (15%)**
- 1-3: Tidak bisa dipahami tanpa 2+ menit konteks sebelumnya
- 4-6: Butuh sedikit setup (1-2 kalimat konteks)
- 7-8: Self-explanatory dengan minimal framing
- 9-10: Bisa di-drop ke TikTok tanpa ANY konteks dan masih make sense

---

## Step 3: Scene Selection

### Selection Strategy

1. **Start from hook**: Pilih segment dengan hook potential tertinggi sebagai opening
2. **Build narrative**: Pilih 3-5 segment yang membentuk arc: hook → setup → core → payoff
3. **Check duration**: Total KEEP segments harus 45-60 detik
4. **Trim if needed**: Jika over-duration, potong dari setup (bukan core atau payoff)

### Common Patterns

| Pattern | Structure | Kapan Digunakan |
|---------|-----------|----------------|
| **Single highlight** | Hook → Context (VO) → Main moment → CTA | Satu moment sangat kuat |
| **Before/After** | Hook → Before state → Transition → After state → CTA | Transformation content |
| **Listicle** | Hook → Item 1 → Item 2 → Item 3 → Best one → CTA | Tips/tricks content |
| **Story arc** | Hook (spoiler) → Start → Complication → Resolution | Narrative content |
| **Tutorial** | Hook (result) → Step 1 → Step 2 → Step 3 → Result | How-to content |

---

## Step 4-5: Cut Plan & Labeling

### KEEP Rules
- Setiap KEEP segment harus punya alasan yang jelas
- Audio source wajib ditentukan (Original / VO / Mix)
- Jika Original: audio quality harus adequate

### CUT Rules
- CUT segment TIDAK masuk final — pastikan tidak ada informasi kritis yang hilang
- Jika CUT segment mengandung konteks penting, rangkum di VO

### GAP Rules
- GAP bukan silence — selalu isi dengan sesuatu:
  - Transition effect (swoosh, dissolve, wipe)
  - Text overlay (stat, quote, key phrase)
  - B-roll suggestion
- Max GAP duration: 2 detik
- Jangan dua GAP berturut-turut

---

## Step 6: VO Script

### Writing Tips

**DO:**
- Tulis seperti ngobrol — "So here's the thing..." bukan "It is important to note that..."
- Gunakan second person — "you" bukan "one" atau "people"
- Front-load value — kalimat pertama = most important info
- Vary sentence length — 3 kata. Lalu kalimat yang sedikit lebih panjang. Lalu singkat lagi.

**DON'T:**
- Jangan mulai dengan "In this video..." atau "Today we're going to..."
- Jangan gunakan passive voice ("was found" → "found")
- Jangan repeat point yang sudah disampaikan
- Jangan tutup dengan "Thanks for watching" (buang 2-3 detik untuk nothing)

### Timing Guide

| Pace | Kata/Detik | Kapan |
|------|-----------|-------|
| Normal | 2.5 | Setup, explanation |
| Fast | 3.5 | Hook, exciting moments |
| Slow | 1.5 | Key point, let it sink in |

Rule of thumb: 45 detik VO ≈ 90-120 kata.

---

## Step 7: Dialog Preservation

### Decision Framework

```
Is audio quality good?
  ├── No → REPLACE WITH VO
  └── Yes
       │
       Is pace appropriate for short-form?
       ├── No (terlalu lambat) → REPLACE WITH VO (kondensasi)
       └── Yes
            │
            Is there genuine emotion?
            ├── Yes → KEEP ORIGINAL
            └── No
                 │
                 Is the specific wording important?
                 ├── Yes (quote, stat, punchline) → KEEP ORIGINAL
                 └── No → Either works — prefer KEEP for authenticity
```

---

## Step 8: Retention Review

### 3-Second Test

Baca hanya 3 detik pertama (hook). Tanyakan:
- Apakah ada pertanyaan yang tercipta di benak viewer?
- Apakah ada promise (implicit atau explicit) yang membuat viewer ingin tahu jawabannya?
- Apakah bisa bersaing dengan video LAIN yang sedang di-scroll?

Jika jawaban ke-3 pertanyaan bukan "ya", hook perlu di-rewrite.

### Engagement Curve

Target engagement curve:

```
Engagement
  ▲
  │   ╱╲        ╱╲
  │  ╱  ╲  ╱╲  ╱  ╲  ╱╲
  │ ╱    ╲╱  ╲╱    ╲╱  ╲
  │╱                     ╲
  └──────────────────────── Time
  0s   15s   30s   45s  60s
  Hook  Setup  Core  Payoff
```

- Tidak boleh ada "valley" yang lebih dari 5 detik
- Setiap 10-15 detik harus ada engagement driver baru
- Ending harus naik (bukan menurun)

### Red Flags

| Red Flag | Fix |
|----------|-----|
| Hook dimulai dengan konteks | Balik — mulai dari payoff/surprise, baru konteks |
| Dead zone > 5 detik | Insert text overlay, cut closer, atau tambah VO energy |
| Ending yang abrupt | Tambah 3-5 detik CTA atau callback ke hook |
| Semua VO, tidak ada original audio | Kurangi VO, tambah original dialog untuk authenticity |
| Total > 60 detik | Cut setup section — viewer bisa survive tanpa it |

---

## Step 9: Metadata

### Title A/B Testing Mindset

Generate 3 opsi title, pilih yang terbaik:
1. **Curiosity gap**: "The AI trick nobody talks about"
2. **Specific number**: "How I debug 10x faster with one prompt"
3. **Relatable pain**: "3 days stuck on a bug. AI found it in 10 seconds."

Pilih berdasarkan:
- Mana yang paling membuat Anda ingin klik?
- Mana yang paling spesifik (bukan generic)?
- Mana yang paling bisa di-deliver oleh konten?

### Hashtag Research

Prioritas hashtag:
1. Check trending hashtags di platform target (jika punya akses)
2. Mix volume: 2 broad (>1B views) + 3 medium (100M-1B) + 3 niche (<100M)
3. Hindari hashtag yang sudah saturated dan irrelevant

---

## Step 10: CapCut Checklist

Checklist di-generate berdasarkan cut plan dan VO script secara spesifik — bukan generic checklist.

### Customization per Content Type

| Content Type | Tambahan Checklist |
|-------------|-------------------|
| Tutorial | Numbered step overlays, zoom ke detail |
| Reaction | Picture-in-picture layout, reaction cam |
| Storytelling | Cinematic bars (letterbox), ambient music |
| Listicle | Progress counter ("1/5", "2/5"), swipe transition |
| Before/After | Split screen moment, dramatic transition |

---

## Partial Pipeline

User bisa request hanya sebagian pipeline:

| Request | Steps yang Dijalankan |
|---------|----------------------|
| "Analisis scene saja" | Step 1-2 |
| "Buat cut plan" | Step 1-5 |
| "VO script saja" | Step 6 (butuh scene context dari user) |
| "Review retention" | Step 8 (butuh existing plan dari user) |
| "Metadata saja" | Step 9 (butuh topic/hook dari user) |
| "Full pipeline" | Step 1-10 |

Untuk partial request, agent harus meminta informasi yang biasanya dihasilkan oleh step sebelumnya.
