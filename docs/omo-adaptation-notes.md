# OMO Adaptation Notes

Catatan ringkas tentang pola dari [Oh-My-OpenAgent](https://github.com/code-yeongyu/oh-my-openagent)
yang **diadaptasi** dan yang **sengaja tidak diadaptasi** ke
`shorts-workflow-skill`.

Filosofi: ambil pola, bukan implementasi. OMO sangat besar (>270K LOC).
Skill ini text-only dan fokus ke satu jenis konten (recap commentary).

---

## Diadaptasi

### 1. Orchestrator + Skill Pattern

OMO memisahkan orchestrator (Sisyphus) dari skill files yang di-load
on-demand. Skill ini meniru pola itu: `SKILL.md` adalah skill file yang
bisa di-load oleh agent orchestrator (MoClaw / OpenClaw / LLM apa pun
via system prompt). Orchestrator yang mengeksekusi pipeline.

### 2. Structured Output dengan Templates

OMO menggunakan structured delegation prompt (TASK / EXPECTED OUTCOME /
MUST DO / MUST NOT DO / CONTEXT). Skill ini meniru: tiap template di
`templates/` mendefinisikan struktur output, rules, dan anti-pattern.

### 3. Verification / Review Loop

OMO punya review-work skill yang menjalankan multiple reviewers paralel.
Skill ini menyederhanakan ke **Step 9 Retention Review** — single-pass
audit dengan checklist tetap (3-second hook, pacing, clarity, twist
timing, ending).

### 4. Opinionated Defaults

OMO menetapkan default model, temperature, dan behavior. Skill ini
opinionated juga: 9:16, 80–110s, US audience, English output, manual
CapCut. User boleh override, tapi default harus "just works".

### 5. Parallelism Hints

OMO Prometheus planner menghasilkan dependency graph + parallel waves.
Skill ini menyertakan **fixed parallelism hint** di SKILL.md (Step 5 + 6
boleh paralel; Step 10 packaging + checklist boleh paralel) — tidak
dihasilkan dinamis, tapi cukup untuk orchestrator yang mau memecah.

---

## Tidak Diadaptasi

### 1. Multi-Model Routing

OMO punya prompt variants per model family (GPT, Claude, Gemini, Kimi).
Tidak diperlukan di sini — instruksi recap commentary cukup sederhana
untuk semua LLM modern. Bisa ditambahkan kalau ada model yang
consistently underperform.

### 2. Hooks System

OMO punya 5-tier hook composition (Session, ToolGuard, Transform,
Continuation, Skill) dengan 50+ hooks. Hooks adalah platform mechanism
(plugin lifecycle). Skill ini hanya file Markdown, tidak perlu lifecycle.

### 3. Team Mode / Multi-Agent Coordination

OMO punya `team_*` tools, mailbox system, per-member git worktrees.
Tidak relevan untuk content workflow yang outputnya satu Markdown
dokumen.

### 4. MCP Integration

OMO punya 3-tier MCP system (built-in HTTP, .mcp.json, skill-embedded).
MCP tidak diperlukan untuk text-only skill. Standard function calling
dari orchestrator sudah cukup.

### 5. Session Recovery / Compaction

OMO punya context window recovery dan preemptive compaction. Output
skill ini ~3–5K token — jauh di bawah limit. Tidak perlu compaction.

### 6. IDE Guards

OMO punya guards untuk file-level safety (writeExistingFileGuard,
hashlineReadEnhancer, dll). Skill ini tidak menulis code, hanya Markdown
plan. Tidak butuh guard.

### 7. Failure Recovery (3x retry → revert → consult Oracle)

OMO punya protocol untuk loop kegagalan code edit. Skill ini tidak
mengubah state. Output adalah generation, kalau kurang bagus user
re-run. Tidak butuh recovery protocol.

---

## Mapping Agent OMO → Step Pipeline

Untuk referensi kalau nanti shorts-workflow dipecah multi-agent:

| OMO Agent | Pipeline Equivalent |
|---|---|
| Sisyphus (orchestrator) | Pipeline controller (Step 1–10) |
| Metis (pre-planning) | Step 1 (Parse Input) + Step 2 (Scene Candidates) |
| Prometheus (planner) | Step 3 (Best Scene) + Step 4 (Story Arc) |
| Hephaestus (deep worker) | Step 5 (VO) + Step 6 (Beat Map) |
| Sisyphus-Junior (executor) | Step 7 (KEEP/CUT/GAP) + Step 8 (Subtitle) |
| Momus (plan reviewer) | Step 9 (Retention Review) |
| Librarian (external research) | Step 10 (Packaging — title/hashtag trend research) |

Tidak wajib dipakai. Single agent yang menjalankan berurutan tetap sah.

---

## Lessons

1. **Ambil pola, bukan implementasi.** OMO terlalu besar untuk direplikasi.
2. **Opinionated defaults > flexible everything.** User yang mau custom
   akan override.
3. **Verification loop** wajib di domain mana pun, tapi formatnya beda
   per domain. OMO cek code quality. Skill ini cek retention.
4. **Templates Markdown lebih natural untuk LLM** dibanding JSON schema.
5. **Single skill file lebih portable** daripada multi-file skill set
   untuk domain yang sempit seperti ini.
