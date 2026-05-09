# VO Script Template

Template untuk menulis voiceover script berdasarkan scene selection dan cut plan.

## Struktur

```markdown
[TIMESTAMP] SECTION_NAME
TYPE: VO / ORIGINAL DIALOG / MIX
---
[Teks yang akan diucapkan/dipertahankan]
```

## Section Flow

### Hook (0-3 detik)

Pilih SATU format:

| Format | Contoh | Kapan Digunakan |
|--------|--------|----------------|
| **Question** | "Have you ever wondered why...?" | Topik yang banyak orang penasaran |
| **Bold claim** | "This changed everything about how I work." | Ada proof/evidence yang kuat |
| **Contrast** | "3 days to find the bug. 10 seconds with AI." | Ada before/after yang dramatis |
| **Challenge** | "Stop doing [X]. Here's why." | Counter-intuitive advice |
| **Story start** | "So yesterday I got this message..." | Narrative-driven content |

Rules:
- Max 15 kata
- Tidak boleh ada setup — langsung ke point
- Harus bisa berdiri sendiri tanpa visual

### Setup (3-15 detik)

- Konteks MINIMAL yang diperlukan viewer
- Jawab: "Kenapa saya harus peduli?" dalam 2-3 kalimat
- Jangan over-explain — biarkan rasa penasaran bekerja

### Core (15-40 detik)

- Isi utama — setiap kalimat harus menambah value
- Gunakan numbered points jika tutorial: "First...", "Second...", "The key thing..."
- Variasi pace: mix VO dengan original dialog

### Payoff / CTA (40-60 detik)

Pilih SATU:

| Type | Format | Contoh |
|------|--------|--------|
| **Takeaway** | Satu kalimat yang bisa di-screenshot | "Give it 2 weeks, not 2 days." |
| **Soft CTA** | Natural next step | "Follow for more [topic] breakdowns." |
| **Loop CTA** | Reference hook → rewatch | "And that's how 3 days became 10 seconds." |
| **Cliffhanger** | Promise part 2 | "But wait until you see what happened on day 7." |

## Template

```
[00:00 - 00:03] HOOK
TYPE: VO
---
"[hook text — max 15 kata]"

[00:03 - 00:04] TRANSITION
TYPE: GAP
---
[transition sound/text overlay: "[text]"]

[00:04 - 00:12] SETUP
TYPE: VO
---
"[setup kalimat 1]"
"[setup kalimat 2]"

[00:12 - 00:27] CORE — SEGMENT A
TYPE: ORIGINAL DIALOG
---
Speaker: "[dialog asli yang dipertahankan]"

[00:27 - 00:40] CORE — SEGMENT B
TYPE: VO
---
"[VO narration]"

[00:40 - 00:50] PAYOFF
TYPE: VO
---
"[payoff/insight text]"

[00:50 - 00:55] CTA
TYPE: ORIGINAL DIALOG
---
Speaker: "[CTA dialog]"
```

## Writing Rules

1. **Max 15 kata per kalimat** — kalau lebih panjang, pecah
2. **Bahasa percakapan** — tulis seperti ngobrol, bukan essay
3. **Hindari jargon** — kecuali audience niche yang paham
4. **Setiap kalimat = value** — jika hapus kalimat dan narasi masih utuh, hapus
5. **Read aloud test** — baca keras, jika terasa janggal, rewrite
6. **Pace variation** — jangan semua VO; mix dengan original dialog untuk authenticity
