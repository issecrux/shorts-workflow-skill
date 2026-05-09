# Template — D. VO Narration Script

Tulis VO third-person narrator dalam **English natural untuk US audience**.
Target 160–230 kata untuk 80–110 detik. Setiap baris bernomor (V01, V02, …)
untuk linking ke Visual Beat Map (E).

## Format

```
### VO Narration Script

- Total kata: <jumlah>
- Estimasi durasi: <detik>
- Tone: <misal: restrained, suspenseful, third-person>
- WPM target: ~120 (dengan jeda dramatik)

| Line ID | Beat Section | Text (English, US-natural) | Target Duration (s) |
|---|---|---|---|
| V01 | Cold open | "She stopped breathing mid-sentence." | 2.5 |
| V02 | Cold open | "And nobody in the room moves." | 2.5 |
| V03 | Setup | "It's a routine call. Two paramedics, one apartment, one woman on the floor." | 5.5 |
| V04 | Setup | "Her partner radios in. The clock starts." | 4.0 |
| ... | ... | ... | ... |
```

Gabungkan semua baris menjadi satu paragraf full-text di bawah tabel
untuk dipakai talent VO:

```
### Full VO Script (for reading)

She stopped breathing mid-sentence. And nobody in the room moves.
It's a routine call. Two paramedics, one apartment, one woman on
the floor. Her partner radios in. The clock starts. ...
```

## Aturan

1. **Third-person narrator.** Bukan karakter. Bukan creator.
2. **Present tense default.** Past tense hanya kalau scene retrospektif.
3. **Kalimat pendek.** Rata-rata 8–14 kata.
4. **Jangan menyalin dialog asli yang panjang.** Boleh quote 1 baris
   pendek (≤8 kata) sangat memorable, tandai dengan kutip.
5. **Jangan klaim "true story" / "based on real events"** kecuali sumber
   non-fiksi.
6. **Jangan mengarang motif/perasaan karakter.** Tetap di permukaan:
   aksi, reaksi, akibat.
7. **Hindari kata berlebihan**: "absolutely insane", "literally
   unbelievable", "you won't believe", "jaw dropping".
8. **Hindari trigger words** untuk kategori sensitif. Lihat
   [`docs/platform-sensitivity.md`](../docs/platform-sensitivity.md).
9. **Word count**: 80s ≈ 160 kata, 95s ≈ 200 kata, 110s ≈ 220–230 kata.

## Anti-pattern

- "This is one of the most insane episodes you'll ever see" — meta
  comment, hapus.
- "She had no idea what was about to happen" — tidak bisa dibuktikan dari
  visual, hapus.
- "Based on a true story" untuk show fiksi — terlarang.
- VO yang merangkum dialog 30 detik menjadi 1 baris hambar — tulis ulang
  dengan stakes yang jelas.

## Contoh Mini

```
### VO Narration Script

- Total kata: 198
- Estimasi durasi: ~95 detik
- Tone: restrained, third-person, present tense

| Line ID | Beat Section | Text | Target (s) |
|---|---|---|---|
| V01 | Cold open | "She stopped breathing mid-sentence." | 2.2 |
| V02 | Cold open | "And nobody in the room moves." | 2.0 |
| V03 | Setup | "It's a routine call. Two paramedics. One apartment." | 5.0 |
| V04 | Setup | "A woman on the floor — and a partner radioing for backup." | 4.5 |
| V05 | Escalation | "Vital signs drop." | 1.8 |
| V06 | Escalation | "Protocol says: stabilize, transport." | 3.0 |
| V07 | Escalation | "Her partner says: there's no time." | 3.0 |
| V08 | Escalation | "She listens. Decides. Moves." | 3.5 |
| V09 | Twist | "Then she hears it." | 2.0 |
| V10 | Twist | "A second voice. Behind the couch." | 3.0 |
| V11 | Twist | "There's a child no one saw." | 3.5 |
| V12 | Payoff | "Two patients. One ambulance. One choice." | 5.0 |
| V13 | Payoff | "She doesn't say why she picked." | 4.0 |

### Full VO Script (for reading)

She stopped breathing mid-sentence. And nobody in the room moves.
It's a routine call. Two paramedics. One apartment. A woman on the
floor — and a partner radioing for backup. Vital signs drop. Protocol
says: stabilize, transport. Her partner says: there's no time. She
listens. Decides. Moves. Then she hears it. A second voice. Behind
the couch. There's a child no one saw. Two patients. One ambulance.
One choice. She doesn't say why she picked.
```
