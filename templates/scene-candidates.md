# Template — A. Scene Candidates

Daftar 3–6 segmen kandidat dari sumber. Tiap kandidat 15–40 detik
(sumber). Skor pakai rubric di [`docs/scoring-rubric.md`](../docs/scoring-rubric.md).

## Format

| # | Timestamp Range | Scene Summary | Category | Hook Potential | Emotional Stakes | Visual Potential | Score | Grade | Reason (Selected / Rejected) |
|---|---|---|---|---|---|---|---|---|---|
| 1 | `00:HH:MM:SS,mmm – 00:HH:MM:SS,mmm` | 1–2 kalimat ringkasan | rescue / trapped victim | Tinggi: ada visual shock di 0–2s | Anak terjebak, ibu di luar | Wajah & lokasi jelas, butuh ~2 insert | 88 | A | Selected — Apex Drop kuat, twist landing di ~70s |

## Aturan

1. **Timestamp range** harus dari sumber, format SRT.
2. **Scene Summary** maksimal 2 kalimat. Sebut role, bukan nama, kecuali
   nama itu sendiri pivotal.
3. **Category** dari daftar scene universal di SKILL.md, atau
   `other: <label>`.
4. **Hook / Emotional / Visual** ditulis singkat (≤12 kata) — kalimat
   atribut, bukan paragraf.
5. **Score** = total dari rubric (Immediate Hook + Understandable +
   Stakes + Visual + Escalation + Twist + VO + Originality + Emotional
   - Platform Risk Penalty).
6. **Grade**: A (85–100), B (70–84), C (55–69), Reject (<55).
7. **Reason** — sebut faktor utama yang menentukan skor + verdict.

## Anti-pattern

- "Lucky scene where everything happens" — terlalu vague, tulis ulang.
- 3 kandidat semua skor 80 — kurang diferensiasi, paksa pembedaan.
- Penalti 0 untuk scene yang melibatkan death / injury / outbreak —
  hampir selalu salah, evaluasi ulang Platform Risk.

## Contoh Mini

```
| # | Timestamp Range | Scene Summary | Category | Hook | Emotion | Visual | Score | Grade | Reason |
|---|---|---|---|---|---|---|---|---|---|
| 1 | 00:12:35,000 – 00:13:02,500 | Paramedic finds a second victim hidden behind a couch — alive | rescue / trapped victim | Visual shock + role reversal | Family stakes, child involved | Tight space, expressive faces | 86 | A | Selected — strong apex + twist |
| 2 | 00:18:11,200 – 00:18:48,000 | Detective realizes the witness is the suspect | crime / investigation | Reveal moment | Moral pivot | Two-shot, restrained acting | 78 | B | Backup — twist solid, but setup heavy |
| 3 | 00:24:50,000 – 00:25:22,800 | Doctor argues with insurance over $10K injection | medical affordability | Stakes via dialogue | Parent-child | Office scene, low visual variety | 64 | C | Reject — talky, low visual potential |
```
