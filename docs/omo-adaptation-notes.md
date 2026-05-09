# OMO Adaptation Notes

Catatan tentang pola dari [Oh-My-OpenAgent](https://github.com/code-yeongyu/oh-my-openagent) yang diadaptasi untuk shorts-workflow-skill, dan apa yang sengaja TIDAK diadaptasi.

---

## Pola yang Diadaptasi

### 1. Orchestrator-Delegate Pattern

**OMO**: Sisyphus (main orchestrator) menerima input → klasifikasi intent → delegate ke specialist agents → verifikasi hasil → compile output.

**Shorts**: Pipeline 10-step mengikuti pola yang sama. Orchestrator menerima transcript → detect input type → jalankan steps → compile final document. Jika orchestrator mendukung delegation, tiap step bisa didelegasikan ke agent terpisah.

**Implementasi**: SKILL.md Section "Tips untuk Orchestrator" menyertakan parallelism hints agar orchestrator bisa memecah pipeline ke multi-agent jika supported.

### 2. Intent Classification (Phase 0)

**OMO**: Sisyphus menjalankan Intent Gate di awal setiap pesan:
- Verbalize intent
- Classify request type (trivial, explicit, exploratory, open-ended, ambiguous)
- Route ke pipeline yang tepat

**Shorts**: Step 1 (Parse Input) berfungsi sebagai intent classification:
- Detect format (.srt vs plain text)
- Detect pipeline mode (full vs partial)
- Route ke steps yang relevan

**Perbedaan**: OMO lebih granular (6 tipe intent). Shorts disederhanakan ke 2 dimensi (input format × pipeline scope) karena domain lebih terbatas.

### 3. Category + Skills System

**OMO**: 8 built-in categories (`visual-engineering`, `ultrabrain`, `deep`, `artistry`, `quick`, dll) dengan skills injection (`load_skills=["frontend-ui-ux"]`).

**Shorts**: Disederhanakan menjadi:
- Categories: `analysis` (scene scoring), `creative-writing` (VO, hooks), `metadata` (title/caption/hashtag)
- Skills: `shorts-workflow` (single skill, semua instruksi di satu file)

**Perbedaan**: OMO punya 10 built-in skills yang masing-masing bisa di-load independently. Shorts menggunakan satu skill karena domain lebih fokus dan steps saling terkait.

### 4. Structured Delegation Prompt

**OMO**: Sisyphus WAJIB menyertakan 6 section dalam setiap delegation:
1. TASK — atomic goal
2. EXPECTED OUTCOME — concrete deliverables
3. REQUIRED TOOLS — tool whitelist
4. MUST DO — exhaustive requirements
5. MUST NOT DO — forbidden actions
6. CONTEXT — file paths, patterns, constraints

**Shorts**: Template di `templates/` mengikuti pola serupa — setiap template mendefinisikan:
- Apa yang harus dihasilkan (struktur tabel/format)
- Kriteria scoring yang explicit
- Rules yang harus diikuti
- Anti-patterns yang harus dihindari

### 5. Verification Loop

**OMO**: review-work skill meluncurkan 5 agent secara paralel untuk review post-implementation (goal verification, QA execution, code review, security audit, context mining).

**Shorts**: Step 8 (Retention Review) berfungsi sebagai verification loop yang disederhanakan:
- 3-second test (hook strength)
- Dead zone check (engagement gaps)
- Pace check (cut duration)
- Ending check (CTA quality)
- Rewatch potential

**Perbedaan**: OMO menggunakan 5 agent paralel. Shorts menggunakan checklist tunggal karena domain review lebih terbatas (retention + compliance, bukan full codebase audit).

### 6. Wave-Based Parallel Execution

**OMO**: Prometheus planner menghasilkan YAML plan dengan dependency graph dan parallel execution waves.

**Shorts**: Pipeline mengidentifikasi parallelism opportunities:
- Wave 1: Step 1-2 (parse + analyze) — sequential
- Wave 2: Step 3-5 (select + cut + label) — sequential
- Wave 3: Step 6 + Step 7 — parallel
- Wave 4: Step 8 — sequential (needs all previous)
- Wave 5: Step 9 + Step 10 — parallel

**Perbedaan**: OMO waves di-generate dinamis per task. Shorts waves fixed karena pipeline selalu sama.

### 7. Default Values yang Opinionated

**OMO**: Setiap agent punya default model, temperature, dan behavior. Sisyphus default ke claude-opus-4-7 max dengan thinking 32k. Explore default ke gpt-5.4-mini-fast (murah, fire liberally).

**Shorts**: Default values yang opinionated:
- Target audience: US, 18-34
- Max duration: 60 detik
- Hook window: 3 detik
- Scoring weights: Hook 25%, Emotion 25%, Info 20%, Visual 15%, Standalone 15%

User bisa override, tapi default harus work out-of-the-box.

---

## Pola yang TIDAK Diadaptasi

### 1. Multi-Model Routing

**OMO**: Setiap agent punya model-specific prompt variants (GPT-5.5, Gemini, Kimi K2.x, Claude Opus 4.7). Prompt di-optimize per model family.

**Mengapa tidak**: Over-engineering untuk v0.1. Skill instruction set sederhana — LLM mana pun bisa mengikutinya tanpa model-specific tuning. Bisa ditambahkan nanti jika ada model yang consistently underperform.

### 2. 52-Hook Lifecycle System

**OMO**: 5-tier hook composition (Session, ToolGuard, Transform, Continuation, Skill) dengan 52-59 hooks.

**Mengapa tidak**: Hooks adalah mekanisme platform (OpenCode plugin system). Shorts-workflow adalah skill file, bukan plugin. Tidak memerlukan lifecycle hooks.

### 3. Team Mode (Parallel Multi-Agent Coordination)

**OMO**: 12 team_* tools, mailbox system, per-member git worktrees, lead/member hierarchy.

**Mengapa tidak**: Terlalu complex untuk content workflow. Background task delegation sudah cukup. Shorts tidak memerlukan concurrent file editing atau worktree isolation.

### 4. MCP Integration

**OMO**: 3-tier MCP system (built-in HTTP, .mcp.json, skill-embedded).

**Mengapa tidak**: MCP adalah AI coding tool protocol. Shorts-workflow tidak memerlukan IDE integration, LSP, atau tool server. Standard function calling dari orchestrator sudah cukup.

### 5. Session Recovery & Compaction

**OMO**: `anthropicContextWindowLimitRecovery`, `preemptiveCompaction`, `compactionTodoPreserver`.

**Mengapa tidak**: Context window management adalah concern orchestrator, bukan skill. Shorts pipeline output (~2-3K tokens) jauh di bawah context limits.

### 6. IDE-Specific Guards

**OMO**: `writeExistingFileGuard`, `hashlineReadEnhancer`, `bashFileReadGuard`, `commentChecker`.

**Mengapa tidak**: Semua ini spesifik untuk code editing safety. Shorts-workflow menghasilkan Markdown document, bukan code.

### 7. Failure Recovery Protocol

**OMO**: Setelah 3x fix gagal → STOP → REVERT → DOCUMENT → CONSULT Oracle.

**Mengapa tidak**: Shorts-workflow tidak mengubah file atau state. Tidak ada "3x gagal" scenario — output selalu fresh generation. Jika output kurang bagus, user bisa re-run dengan feedback.

### 8. Canonical Agent Sort Shim

**OMO**: `installAgentSortShim()` — patches `Array.prototype.sort` untuk memastikan agent order.

**Mengapa tidak**: Single skill, tidak ada multi-agent ordering concern.

---

## Mapping Agent OMO → Shorts Workflow

Untuk referensi jika nanti shorts-workflow dipecah menjadi multi-agent:

| OMO Agent | Shorts Equivalent | Notes |
|-----------|-------------------|-------|
| Sisyphus (orchestrator) | Pipeline controller | Koordinasi 10 steps |
| Explore (codebase search) | — | Tidak ada codebase untuk di-search |
| Librarian (external docs) | Trend research step | Riset trending audio/hashtags |
| Oracle (consultant) | — | Tidak ada arsitektur decision |
| Momus (plan reviewer) | Retention Reviewer (Step 8) | Review plan, bukan code |
| Prometheus (planner) | — | Pipeline sudah pre-defined |
| Metis (pre-planning) | Input Classifier (Step 1) | Classify input sebelum proses |
| Atlas (todo orchestrator) | — | Pipeline linear, tidak butuh todo |
| Hephaestus (deep worker) | VO Writer (Step 6) | Deep creative work |
| Sisyphus-Junior (executor) | Per-step executor | Eksekusi step individual |
| Multimodal-Looker | — | v0.1 text-only, no visual input |

---

## Lessons Learned

1. **OMO sangat mature** (278K LOC, 1967 files) — jangan coba replikasi semua. Ambil pola, bukan implementasi.
2. **Intent classification adalah pattern paling universal** — berlaku di domain apa pun, bukan hanya coding.
3. **Delegation prompt structure (6-section)** mengurangi ambiguitas drastis — worth implementing di domain mana pun.
4. **Verification loop** perlu disesuaikan per domain — OMO cek code quality, Shorts cek retention. Prinsipnya sama: verify sebelum deliver.
5. **Opinionated defaults > flexible everything** — user yang butuh customization akan override. User yang baru mulai butuh "just works" experience.
