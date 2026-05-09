# Workflow — Detail Per Step

Dokumen ini menjelaskan eksekusi tiap step di pipeline `shorts-workflow-skill`,
termasuk input/output yang diharapkan, edge case, dan tips kualitas.

Pipeline berisi 10 step yang menghasilkan output A–J. Eksekusi default
sekuensial, tapi step 5 dan 6 boleh diparalelkan setelah step 4 selesai.

---

## Step 1 — Parse Input

**Tujuan:** Normalkan input menjadi tabel segment yang konsisten.

**Input:**

- `.srt` lengkap dengan timestamp, atau
- Plain transcript tanpa timestamp.

**Output internal (tidak ditampilkan ke user):**

```
[
  { index: 1, start: "00:00:01,000", end: "00:00:04,500", text: "..." },
  ...
]
```

**Catatan:**

- Jika input plain text, segmentasikan per kalimat. Estimasi durasi pakai
  ~150 wpm (sekitar 0.4 detik per kata). Tandai semua timestamp sebagai
  estimasi (misal: `~00:00:14`).
- Catat total durasi sumber.
- Jika sumber < 60 detik → return error (lihat SKILL.md error handling).

**Edge cases:**

- SRT dengan multiple speakers: catat speaker tag jika ada.
- SRT dengan format non-standard (timestamps `.` bukan `,`): normalkan.
- Karakter non-ASCII: pertahankan apa adanya.

---

## Step 2 — Scene Candidates (Output A)

**Tujuan:** Identifikasi 3–6 segmen kandidat yang berpotensi jadi short.

**Input:** Hasil parse dari Step 1.

**Cara memilih kandidat:**

1. Cari "peak moments" — perubahan stakes, reveal, ledakan emosi, momen
   diam yang berat.
2. Tiap kandidat berdurasi 15–40 detik (sumber). Bukan satu subtitle
   line.
3. Beri 1–2 kalimat ringkasan scene.
4. Klasifikasikan ke salah satu **kategori scene universal** (lihat
   SKILL.md). Boleh `other: <label>` jika tidak cocok.

**Scoring:** Pakai rubric di
[`scoring-rubric.md`](scoring-rubric.md). Total 100 dengan penalti
Platform Risk -10..0.

**Output:** Tabel mengikuti
[`templates/scene-candidates.md`](../templates/scene-candidates.md).

**Tips kualitas:**

- Jangan pilih scene yang butuh konteks lebih dari 1 kalimat untuk
  dimengerti.
- Jangan beri skor tinggi pada scene yang hanya "lucu" tanpa stakes.
- Skor ≥85 = Grade A. Skor 70–84 = Grade B (butuh VO kuat). 55–69 =
  Grade C (lemah). <55 = Reject.

---

## Step 3 — Best Scene (Output B)

**Tujuan:** Pilih satu kandidat dengan skor tertinggi yang bisa berdiri
sebagai short 80–110 detik.

**Input:** Tabel kandidat dari Step 2.

**Aturan pemilihan:**

- Kandidat dengan skor tertinggi **bukan otomatis pemenang**.
- Tolak kandidat dengan Platform Risk -8..-10 jika ada alternatif Grade
  B+ tanpa risiko tinggi.
- Pilih kandidat yang `core mystery`-nya bisa di-tease dalam 3 detik.

**Output:** Mengikuti [`templates/best-scene.md`](../templates/best-scene.md):

- chosen timestamp range
- why this scene wins
- core conflict
- core mystery
- emotional payoff
- ideal duration

---

## Step 4 — Story Arc (Output C)

**Tujuan:** Susun arc 80–110 detik yang punya struktur jelas.

**Struktur default:**

| Beat | Window | Fungsi |
|---|---|---|
| Cold open (Apex Drop) | 0–5s | Hook visual + VO satu kalimat |
| Setup | 5–20s | Konteks minimal, sebut role bukan nama |
| Escalation | 20–55s | Tingkatkan stakes setiap ~10s |
| Twist / Reveal | 55–85s | Pembalikan asumsi |
| Payoff / Cliffhanger | 85–110s | Closure atau tease part 2 |

**Output:** Mengikuti [`templates/story-arc.md`](../templates/story-arc.md).

**Edge cases:**

- Jika twist sumber terjadi sangat awal, **boleh** restructure: simpan
  twist di tengah, isi cold open dengan akibatnya.
- Jika scene tidak punya twist (mis. murni emotional reveal), gunakan
  *emotional pivot* sebagai pengganti twist.

---

## Step 5 — VO Narration Script (Output D)

**Tujuan:** Tulis VO 160–230 kata yang mengisi 80–110 detik.

**Aturan:**

- English natural untuk US audience.
- Third-person narrator.
- Present tense default.
- Setiap baris VO bernomor (V1, V2, …) untuk linking ke beat map.
- Jangan menyalin dialog asli yang panjang. Boleh quote 1 baris pendek
  sangat memorable.
- Jangan klaim "true story" jika sumber fiksi.
- Jangan mengarang motif/perasaan karakter.

**Output:** Mengikuti
[`templates/vo-script.md`](../templates/vo-script.md). Tabel
`(line_id, text, target_duration_seconds, beat_section)`.

**Word count guideline:**

- 80s short ≈ 160 kata
- 95s short ≈ 200 kata
- 110s short ≈ 220–230 kata

Mengasumsikan VO talent membaca ~120 wpm dengan jeda dramatik.

---

## Step 6 — Visual Beat Map (Output E)

**Tujuan:** Petakan tiap baris VO ke timestamp visual sumber.

**Output:** Tabel mengikuti
[`templates/visual-beat-map.md`](../templates/visual-beat-map.md):

| beat | vo line id | source timestamp | visual function | cut note | pacing note |

**Rules:**

- Tiap VO line minimal punya 1 visual. Boleh punya beberapa visual yang
  cepat berturut-turut.
- "visual function" = label deskriptif (mis. `establish location`,
  `reveal injury`).
- "cut note" = jenis transisi: `hard cut`, `cross-fade`, `freeze`,
  `slow-mo`, `whip pan`.
- "pacing note" = `fast (≤1.5s)`, `medium (1.5–3s)`, `slow (>3s)`.
- Boleh **repeat visual** untuk emphasis (slow-mo, freeze), tapi jangan
  loop satu klip lebih dari 2x.

**Tips:**

- Cek konsistensi: VO line ke-N punya total durasi visual ≈ durasi VO?
- Hindari talking head terus-menerus. Selipkan reaction shot, B-roll,
  insert.

---

## Step 7 — KEEP / CUT / GAP Plan (Output F)

**Tujuan:** Tentukan visual mana yang dipertahankan, dibuang, atau perlu
dijembatani VO.

**Output:** Mengikuti
[`templates/keep-cut-gap.md`](../templates/keep-cut-gap.md). Tiga tabel:

- **KEEP** — `[source_start, source_end, reason]`
- **CUT** — `[source_start, source_end, reason_to_cut]`
- **GAP** — `[gap_position_in_short, narrative_purpose, vo_line_id_yang_jembatani]`

**Diagnostik:**

- Kalau total KEEP < 60s → mungkin terlalu agresif memotong, cek lagi.
- Kalau total KEEP > 110s → harus dipotong lebih ketat.
- Setiap GAP **wajib** punya VO line yang menutupnya. Kalau tidak, bukan
  GAP — itu plot hole.

---

## Step 8 — Subtitle Plan (Output G)

**Tujuan:** Pecah VO menjadi chunk subtitle mobile-friendly.

**Aturan:**

- 3–5 kata per chunk (max 6).
- Hindari memecah preposisi dari nounnya ("at the / hospital" buruk).
- Sinkronkan ke baris VO (V1, V2, …).
- Tandai 1–2 kata kunci per chunk untuk **highlight** (bold / warna
  beda).

**Saran style (untuk CapCut):**

- All caps optional. Default: sentence case dengan keyword highlight.
- Font: bold sans-serif (mis. CapCut "Komika Axis", "Anton", atau "Bebas
  Neue").
- Stroke / shadow: hitam, supaya readable di background apa pun.
- Posisi: 1/3 atas atau tengah, hindari menutupi wajah.

**Output:** Tabel langsung di hasil akhir (template embedded di SKILL.md):

```
| chunk_id | vo_line_id | text | highlight |
|----------|------------|------|-----------|
| S01      | V01        | "A patient flatlines" | flatlines |
```

---

## Step 9 — Retention Review (Output H)

**Tujuan:** Self-audit sebelum export ke produksi.

**Checklist:**

| Check | Lulus jika |
|---|---|
| 3-second hook | Cold open punya gambar + VO yang langsung memicu pertanyaan |
| Pacing risk | Tidak ada gap > 4 detik tanpa beat baru |
| Clarity | Penonton US tanpa konteks tetap paham |
| Twist timing | Twist landing di window 55–85s |
| Ending effectiveness | Payoff / cliffhanger jelas, bukan fade-to-nothing |

**Verdict:** `PASS` / `NEEDS WORK`. Kalau `NEEDS WORK`, sertakan saran
spesifik (mis. "twist landing terlalu cepat di 38s, geser ke ~70s").

---

## Step 10 — Packaging + CapCut Checklist (Output I & J)

### I. Packaging

Mengikuti [`templates/packaging.md`](../templates/packaging.md):

- 10 YouTube Shorts titles
- 5 TikTok captions
- Hashtag set
- Pinned comment idea
- Part 2 hook (jika ada material)

**Aturan title/caption:**

- English natural untuk US audience.
- Default **tanpa emoji**. Pakai emoji hanya kalau benar-benar menambah
  daya tarik (max 1 per title/caption).
- Hindari title yang spammy ("YOU WONT BELIEVE!!!").
- Bedakan tone YouTube Shorts (lebih clean) vs TikTok (lebih playful
  / native).

### J. CapCut Editing Checklist

Mengikuti
[`templates/capcut-checklist.md`](../templates/capcut-checklist.md):

- opening frame
- subtitle style
- zoom/crop notes (9:16 reframing dari source 16:9)
- SFX cues
- music / pacing notes
- cut timing
- ending frame

---

## Eksekusi Praktis

### Single-pass (default)

Agent menjalankan Step 1 → 10 dalam satu generation. Output: satu
Markdown lengkap berisi A–J.

### Iterative

User boleh meminta revision per bagian:

- "Re-do scene candidates, beri lebih banyak alternatif Grade B."
- "VO terlalu lebay, tulis ulang lebih restrained."
- "Twist landing di 40s, geser ke ~70s."

Agent re-run step yang relevan tanpa harus re-run semua.

### Partial

User skip sebagian step:

- "Skip scoring, gue udah tau scene mana. Langsung VO + Visual Beat Map."
- "Cuma butuh packaging dari plan ini." → input adalah hasil A–H sebelumnya.

Selalu hormati permintaan partial. Jangan paksa pipeline penuh kalau
tidak diminta.
