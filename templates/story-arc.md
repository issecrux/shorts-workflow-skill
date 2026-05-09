# Template — C. Story Arc

Susun arc 80–110 detik dengan 5 beat: cold open, setup, escalation,
twist/reveal, payoff/cliffhanger.

## Format

```
### Story Arc

| Beat | Window | Source Timestamp | Function | Notes |
|---|---|---|---|---|
| Cold open (Apex Drop) | 0–5s | <source ts> | Hook visual + 1-line VO | <visual & ?> |
| Setup | 5–20s | <source ts> | Konteks minimal | <role names, lokasi> |
| Escalation | 20–55s | <source ts> | Tingkatkan stakes | <beats kenaikan> |
| Twist / Reveal | 55–85s | <source ts> | Pembalikan asumsi | <reveal & visual cue> |
| Payoff / Cliffhanger | 85–110s | <source ts> | Closure / tease part 2 | <ending visual> |
```

## Aturan

1. **Window** = posisi di short (bukan di sumber).
2. **Source Timestamp** = posisi di sumber. Boleh lebih dari satu range
   per beat karena visual dari beat ini bisa diambil dari beberapa
   tempat di sumber.
3. **Function** = peran beat ini di arc.
4. **Notes** = detail teknis yang membantu Step 5 (VO) dan Step 6 (Visual
   Beat Map).

## Edge case

- **Twist sumber terjadi sangat awal**: tetap susun arc dengan twist di
  window 55–85s. Cold open boleh diambil dari **akibat twist**, lalu
  arc menjelaskan bagaimana sampai di sana.
- **Tidak ada twist** (murni emotional reveal): pakai *emotional pivot*
  sebagai pengganti — momen di mana stakes berubah secara emosional
  meskipun tidak ada reveal logis.
- **Tidak ada cliffhanger** (scene tertutup): payoff = closure jelas,
  bukan dipaksa menjadi cliffhanger.

## Contoh Mini

```
### Story Arc

| Beat | Window | Source Timestamp | Function | Notes |
|---|---|---|---|---|
| Cold open | 0–4s | 00:12:35,000 – 00:12:38,500 | Paramedic freezes mid-step inside the apartment | Tight close-up + VO: "She stopped breathing." |
| Setup | 4–18s | 00:12:38,500 – 00:12:50,000 | Establish scene: woman on floor, partner calling for backup | Wide shot, then cut to face |
| Escalation | 18–55s | 00:12:50,000 – 00:13:02,500 + 00:13:30,000 – 00:13:50,000 | Vital signs drop, partner argues for a different protocol | Insert dari 00:13:32 (monitor flatlines) |
| Twist | 55–82s | 00:14:05,000 – 00:14:18,000 | Behind the couch — second victim, still breathing | Slow pan, freeze on hand |
| Payoff | 82–98s | 00:14:18,000 – 00:14:30,000 | Paramedic chooses, but doesn't say why | Cliffhanger: monitor cuts to black |
```
