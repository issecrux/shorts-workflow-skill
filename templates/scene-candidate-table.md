# Scene Candidate Table

Gunakan tabel ini untuk scoring setiap segment dari transcript/subtitle.

## Instruksi

1. Segmentasi source per ~15 detik (atau per natural break jika plain text)
2. Scoring tiap segment berdasarkan 5 kriteria (1-10)
3. Hitung weighted total
4. Sort descending, pilih top candidates

## Kriteria Scoring

| Kriteria | Bobot | 1-3 (Low) | 4-6 (Mid) | 7-10 (High) |
|----------|-------|-----------|-----------|--------------|
| **Hook potential** | 25% | Informatif tapi datar | Ada element menarik | Langsung memicu curiosity/emotion |
| **Emotional peak** | 25% | Netral, factual | Ada humor/tension ringan | Surprise, strong reaction, revelation |
| **Info density** | 20% | Mostly filler/transition | Mix content + filler | Setiap detik = value baru |
| **Visual potential** | 15% | Talking head saja | Ada gesture/screen share | Action, transformation, reveal |
| **Standalone clarity** | 15% | Butuh banyak konteks | Butuh sedikit konteks | Self-explanatory |

## Template

```markdown
| # | Timestamp | Ringkasan | Hook | Emotion | Info Density | Visual | Standalone | **Total** |
|---|-----------|-----------|------|---------|--------------|--------|------------|-----------|
| 1 | 00:00:00 - 00:00:15 | [ringkasan singkat] | ?/10 | ?/10 | ?/10 | ?/10 | ?/10 | **?.?** |
| 2 | 00:00:15 - 00:00:30 | [ringkasan singkat] | ?/10 | ?/10 | ?/10 | ?/10 | ?/10 | **?.?** |
```

## Perhitungan Total

```
Total = (Hook × 0.25) + (Emotion × 0.25) + (InfoDensity × 0.20) + (Visual × 0.15) + (Standalone × 0.15)
```

## Threshold

- **Score >= 7**: Strong candidate — prioritaskan
- **Score 5-6.9**: Usable — pertimbangkan jika mendukung narasi
- **Score < 5**: Likely CUT — hanya gunakan jika benar-benar diperlukan untuk konteks
