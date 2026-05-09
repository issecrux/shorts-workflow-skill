# Template — A. Scene Candidates

Daftar 3–6 **candidate story unit** dari sumber. Skor pakai rubric di
[`docs/scoring-rubric.md`](../docs/scoring-rubric.md).

> **Definisi — Candidate Story Unit:** sebuah sequence dari sumber yang
> punya cukup struktur naratif untuk berdiri sebagai recap-commentary
> short standalone. Boleh terdiri dari beberapa subtitle block
> berurutan, atau sequence yang dikompres dari timestamp berdekatan.

Aturan ukuran:

- **Source duration kandidat: 60–180 detik** (biasanya). Bukan satu
  pertukaran dialog pendek.
- **Apex hook segment di dalamnya: 5–20 detik** (akan jadi cold open).
- Final short tetap 80–110 detik.
- Kandidat harus punya cukup material untuk arc lengkap: cold open,
  setup, escalation, twist/reveal, payoff/cliffhanger.

## Format

| # | Story Unit Range | Apex Hook Segment | Scene Summary | Category | Hook Potential | Emotional Stakes | Visual Potential | Score | Grade | Reason (Selected / Rejected) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | `00:HH:MM:SS,mmm – 00:HH:MM:SS,mmm` (≈90s) | `00:HH:MM:SS,mmm – 00:HH:MM:SS,mmm` (≈12s) | 1–2 kalimat ringkasan unit | rescue / trapped victim | Tinggi: visual shock di apex 0–2s | Anak terjebak, ibu di luar | Wajah & lokasi jelas, butuh ~2 insert | 88 | A | Selected — Apex Drop kuat, twist landing di ~70s |

## Aturan

1. **Story Unit Range** — full sequence sumber (60–180s biasanya),
   format SRT.
2. **Apex Hook Segment** — sub-segment 5–20s di dalam Story Unit Range
   yang paling tegang/fatal/membingungkan. Wajib subset dari kolom
   sebelumnya.
3. **Scene Summary** maksimal 2 kalimat. Sebut role, bukan nama,
   kecuali nama itu sendiri pivotal.
4. **Category** dari daftar scene universal di SKILL.md, atau
   `other: <label>`.
5. **Hook / Emotional / Visual** ditulis singkat (≤12 kata) — kalimat
   atribut, bukan paragraf.
6. **Score** = total dari rubric (Immediate Hook + Understandable +
   Stakes + Visual + Escalation + Twist + VO + Originality + Emotional
   - Platform Risk Penalty).
7. **Grade**: A (85–100), B (70–84), C (55–69), Reject (<55).
8. **Reason** — sebut faktor utama yang menentukan skor + verdict.

## Anti-pattern

- Story unit hanya 15–30 detik — itu apex hook segment, bukan story
  unit. Perluas range sampai dapat arc lengkap (60–180s).
- Apex hook > 25 detik — terlalu panjang sebagai cold open. Ringkas ke
  beat paling kuat.
- "Lucky scene where everything happens" — terlalu vague, tulis ulang.
- 3 kandidat semua skor 80 — kurang diferensiasi, paksa pembedaan.
- Penalti 0 untuk kandidat yang melibatkan death / injury / outbreak —
  hampir selalu salah, evaluasi ulang Platform Risk.

## Contoh Mini

```
| # | Story Unit Range | Apex Hook Segment | Scene Summary | Category | Hook | Emotion | Visual | Score | Grade | Reason |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | 00:11:50,000 – 00:13:35,000 (≈105s) | 00:12:38,000 – 00:12:50,000 (≈12s) | Paramedic finds a second victim hidden behind a couch — alive | rescue / trapped victim | Visual shock + role reversal | Family stakes, child involved | Tight space, expressive faces | 86 | A | Selected — strong apex + twist + arc lengkap |
| 2 | 00:17:30,000 – 00:19:20,000 (≈110s) | 00:18:11,200 – 00:18:25,000 (≈14s) | Detective realizes the witness is the suspect | crime / investigation | Reveal moment | Moral pivot | Two-shot, restrained acting | 78 | B | Backup — twist solid, butuh 1 line VO untuk setup |
| 3 | 00:24:10,000 – 00:25:55,000 (≈105s) | 00:24:50,000 – 00:25:05,000 (≈15s) | Doctor argues with insurance over $10K injection | medical affordability | Stakes via dialogue | Parent-child | Office scene, low visual variety | 64 | C | Reject — talky, low visual potential |
```
