# Template — F. KEEP / CUT / GAP Plan

Tentukan visual sumber mana yang dipertahankan (KEEP), dibuang (CUT),
dan bagian yang harus dijembatani VO (GAP).

## Format

```
### KEEP / CUT / GAP Plan

#### KEEP

| Source Start | Source End | Reason |
|---|---|---|
| 00:12:38,000 | 00:12:42,000 | Apex Drop visual (paramedic freeze) |
| 00:12:43,000 | 00:12:50,000 | Establish location |
| 00:13:00,500 | 00:13:11,500 | Vital drop + protocol argument |
| 00:14:05,000 | 00:14:13,500 | Twist reveal |
| 00:14:18,000 | 00:14:27,000 | Payoff |

#### CUT

| Source Start | Source End | Reason to Cut |
|---|---|---|
| 00:12:50,000 | 00:13:00,500 | Slow walking, no plot value |
| 00:13:11,500 | 00:14:05,000 | Lengthy backstory dialog, kill pacing |
| 00:13:55,000 | 00:14:00,000 | Establishing shot duplicate |

#### GAP

| Gap Position (in short) | Narrative Purpose | Bridged by VO Line |
|---|---|---|
| 18s–25s | Penonton butuh tahu kenapa partner panik | V04 |
| 50s–55s | Transisi dari escalation ke twist | V09 |
| 95s–98s | Ending butuh weight emosional, visual sumber kurang | V13 |
```

## Aturan

1. **KEEP**:
   - Visual yang **memajukan cerita** atau **menjelaskan stakes**.
   - Total durasi KEEP idealnya 60–90s di sumber (untuk menghasilkan
     short 80–110s setelah ditambah VO + insert).
2. **CUT**:
   - Filler, dialog panjang yang merangkum (bisa diganti VO 1 baris),
     establishing shot duplicat, dead air.
   - Cantumkan **alasan spesifik**, bukan generic "boring".
3. **GAP**:
   - Setiap GAP **wajib** punya VO line yang menjembataninya.
   - Kalau tidak ada VO yang menutup, ini bukan GAP — itu plot hole.
     Kembali ke Step 5, tambahkan VO line.

## Diagnostik

- KEEP < 60s di sumber → mungkin terlalu agresif memotong, cek lagi.
- KEEP > 110s di sumber → akan jadi short > 110s setelah ditambah jeda
  VO; potong lebih ketat.
- Setiap CUT besar (>15s) → konfirmasi GAP-nya tertutup VO.

## Anti-pattern

- KEEP semua, CUT kosong → bukan editing, hanya playback.
- CUT besar tanpa GAP → penonton kebingungan.
- GAP tanpa VO line → plot hole.

## Contoh Pemeriksaan Cepat

Total source duration: 2 menit 5 detik.
KEEP total: 73 detik.
CUT total: 52 detik.

Setelah VO + insert, short final: ~98 detik. Aman di range 80–110.
