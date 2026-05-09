# Template — E. Visual Beat Map

Petakan tiap baris VO ke timestamp visual sumber. Tujuan: tidak ada VO
line tanpa visual pendukung.

## Format

```
### Visual Beat Map

| Beat # | VO Line ID | Source Timestamp | Visual Function | Cut Note | Pacing Note |
|---|---|---|---|---|---|
| B01 | V01 | 00:12:38,000 – 00:12:40,200 | Establish: paramedic frozen mid-step | Hard cut from black | Fast (≤1.5s) |
| B02 | V02 | 00:12:40,200 – 00:12:42,000 | Reaction: room stillness | Cross-fade | Slow (>3s) |
| B03 | V03 | 00:12:43,000 – 00:12:48,500 | Wide shot of apartment | Hard cut | Medium (1.5–3s) |
| ... | ... | ... | ... | ... | ... |
```

## Aturan

1. **Setiap VO line harus punya minimal 1 visual.** Kalau tidak ada,
   rewrite VO.
2. **Visual Function** — label deskriptif: `establish location`,
   `reveal injury`, `victim reaction`, `protocol moment`,
   `equipment shot`, `time pressure`, `twist beat`, `aftermath`.
3. **Cut Note** — jenis transisi:
   - `hard cut` — instan, default.
   - `cross-fade` — untuk emosi.
   - `freeze` — pause di frame, default 0.5–1s.
   - `slow-mo` — 0.5x atau 0.25x.
   - `whip pan` — transisi cepat dengan motion blur.
   - `match cut` — cut pada gerakan / shape yang sama.
4. **Pacing Note**:
   - `fast` (≤1.5s)
   - `medium` (1.5–3s)
   - `slow` (>3s)
5. **Repeat visual** boleh untuk emphasis (slow-mo + freeze), tapi
   jangan loop satu klip lebih dari 2x.
6. **Total durasi visual ≈ durasi VO line.** Boleh sedikit lebih panjang
   (max 0.5s overrun) untuk hold dramatik di akhir.

## Anti-pattern

- VO line tanpa visual → bukan beat map.
- 4 klip identik berurutan → terdeteksi sebagai filler.
- Talking head 90 detik tanpa insert → tidak engaging.
- Cut note "hard cut" untuk semua → boring, variasi dibutuhkan.

## Contoh Mini

```
### Visual Beat Map

| Beat # | VO Line ID | Source Timestamp | Visual Function | Cut Note | Pacing Note |
|---|---|---|---|---|---|
| B01 | V01 | 00:12:38,000 – 00:12:40,200 | Paramedic frozen mid-step | Hard cut from black | Fast |
| B02 | V02 | 00:12:40,200 – 00:12:42,000 | Wide shot, room stillness | Cross-fade | Slow |
| B03 | V03 | 00:12:43,000 – 00:12:48,500 | Establish apartment | Hard cut | Medium |
| B04 | V04 | 00:12:50,000 – 00:12:54,500 | Partner on radio | Hard cut | Medium |
| B05 | V05 | 00:13:00,500 – 00:13:02,000 | Monitor: vitals drop | Match cut | Fast |
| B06 | V06 | 00:13:02,000 – 00:13:05,000 | Insert: protocol card | Hard cut | Fast |
| B07 | V07 | 00:13:05,000 – 00:13:08,000 | Partner argues, close-up | Hard cut | Medium |
| B08 | V08 | 00:13:08,000 – 00:13:11,500 | Paramedic moves toward woman | Hard cut | Medium |
| B09 | V09 | 00:14:05,000 – 00:14:07,000 | Sound cue: faint breathing | Slow-mo | Slow |
| B10 | V10 | 00:14:07,000 – 00:14:10,000 | Pan toward couch | Whip pan | Medium |
| B11 | V11 | 00:14:10,000 – 00:14:13,500 | Reveal: small hand under blanket | Freeze | Slow |
| B12 | V12 | 00:14:18,000 – 00:14:23,000 | Two-shot: woman + child | Hard cut | Medium |
| B13 | V13 | 00:14:23,000 – 00:14:27,000 | Ambulance doors close, no dialog | Cross-fade to black | Slow |
```
