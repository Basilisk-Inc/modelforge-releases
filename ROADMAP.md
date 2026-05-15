# Roadmap

Where Spinalis is going. Dates are intent, not promises — solo development means scope shifts when reality demands it. This document is updated with every release.

For "what's already shipped," see [`CHANGELOG.md`](CHANGELOG.md). For a fuller view of what's confirmed working today, see [Status in the README](README.md#status).

---

## Now — v0.0.1 (May 2026)

The shipped foundation. Documented here so you know where the line between "works today" and "coming soon" sits.

**Fine-tuning & training**
- ✅ QLoRA (4-bit) and LoRA (bf16) on text models
- ✅ Pre-validated families: Qwen 2.5 (0.5B–7B), Phi-3.5 / Phi-4, Gemma 2 / 3, Florence-2, Whisper
- ✅ Custom model addition via Hugging Face URL (paste `org/repo`, auto-fetch the card)

**Serving & integration**
- ✅ OpenAI-compatible chat completions (FastAPI runtime)
- ✅ Optional session persistence via the `X-Spinalis-Session-Id` header
- ✅ Streaming responses, including reasoning blocks for R1-style models

**Data flow**
- ✅ Dataset analyzer: schema, statistics, quality issues, model-compatibility tags
- ✅ Train/eval split with stratified sampling
- ✅ Chat playground with streaming + reasoning controls
- ✅ Chat history persistence (in-app + via API header)
- ✅ Correction-driven datasets — thumbs up/down → next training round, exportable as SFT and DPO formats

**Platform**
- ✅ NVIDIA training & inference (driver 535+, CUDA 12.1 wheels)
- ✅ Apple Silicon (MPS) training & inference — runtime works, UI label is wrong (see [Known issues](#known-issues-in-v001))
- ✅ Per-GPU licensing with online and **offline activation** (export fingerprint → receive signed license file)
- ✅ Air-gapped installs as a first-class case
- ✅ CLI installers on all four platforms: Linux + macOS `install.sh`, Windows `install.ps1`
- ✅ Bundled Python 3.12 — no system Python required, your existing envs untouched
- ✅ In-app documentation

### Known issues in v0.0.1

- ❗ **Windows Defender false positives.** Unsigned + obfuscated binary triggers Wacatac.B!ml on a meaningful fraction of machines. Workaround documented in [README → Windows Defender / SmartScreen](README.md#windows-defender--smartscreen); fix (EV cert) is in v0.1.
- ❗ **Apple Silicon shows "CPU-only" in the Resources page** even when MPS is active. Training and inference work correctly — only the label is wrong. Fix is in v0.1.
- ❗ **NVIDIA + Linux Secure Boot** requires MOK enrollment before `nvidia-smi` works. Documented in `docs/01-installation.md`, no fix needed in product.

---

## Q4 2026 — v0.1.x

**Theme: Distribution polish & beyond SFT.** v0.0.1 proved the foundation works. v0.1 makes it deployable without the install dance, and expands what kinds of training you can do.

### Distribution & install — finishing the rough edges
- **Inno Setup wizard for Windows** — double-clickable `.exe` installer with per-user / per-machine selection, data-directory picker, Python source picker (bundled vs system), uninstaller
- **macOS `.pkg`** for Apple Silicon and Intel via `pkgbuild` + `productbuild`, with the standard Installer.app flow
- **EV code-signing certificate** for Windows binaries → no more SmartScreen warnings, no more Defender quarantine on extraction. This is the single biggest UX improvement for any customer in finance / defense / government, where unsigned binaries fail security review automatically.
- **Apple Developer ID + notarization** for the macOS `.pkg` → no more "right-click → Open" dance, no more Gatekeeper warning

### Apple Silicon — full integration
- Surface MPS correctly in the Resources page (Go-side GPU discovery learns about Apple's `system_profiler` / `ioreg`)
- License enforcement on MPS (single-GPU equivalent — Apple Silicon counts as one GPU)

### DPO and preference fine-tuning
- DPO trainer with the existing `jsonl_dpo` storage format already in place
- Correction flow extended: thumbs-down → "what should it have said" → DPO record
- Reference model selection (frozen base vs different SFT adapter)

### Better dataset tooling
- Dataset deduplication (currently only counts duplicates; next iteration removes them)
- Dataset diff view — what changed between v1 and v2 of the same data
- Bulk correction import from CSV (for teams already labeling outside Spinalis)

### Diagnostic endpoint
- `GET /api/v1/system/diagnostics` returning Python path, Python version, huggingface_hub version, OS, sanitized PATH, GPU info — one-command "what's wrong with my install?" reports for support tickets

---

## Q1 2027 — v0.2.x

**Theme: More GPU vendors & reasoning.** Open up beyond NVIDIA-only.

### AMD ROCm support
- Linux + AMD GPU detection and `rocm-smi` integration on the Go side
- PyTorch ROCm wheel installation in `install.sh`
- License enforcement extended to ROCm GPU UUIDs
- Training & inference validated on at least one consumer (RX 7900 XTX) and one datacenter (MI300) card

### DirectML for Windows AMD GPUs
- `torch-directml` integration as fallback when no NVIDIA is present
- Note: DirectML is slower than ROCm; documented as a "make it work first" path for Windows users with AMD GPUs

### GRPO and reasoning training
- GRPO trainer for reasoning-heavy tasks (math, code, multi-step)
- `train_on_reasoning: true` already in the config — wire to a proper reasoning-aware loss
- Reasoning-trace dataset templates for distilling R1-style models into smaller targets

### Evaluation harness
- BLEU / ROUGE / exact-match for SFT
- Win-rate eval for DPO (paired-comparison against base model)
- Per-record annotated outputs viewable in the UI

---

## Q2 2027 — v0.3.x

**Theme: Multi-modal.** Text-only is the cheap part. This quarter brings vision and audio fine-tuning end to end.

### Vision fine-tuning
- LLaVA-1.5 / 1.6 architecture support
- Florence-2 fine-tuning (caption, detect, OCR head retraining)
- Image-grounded chat in the playground (drag-drop image → multi-modal query)
- Image-history viewer — vision equivalent of today's chat history page

### Audio fine-tuning
- Whisper fine-tuning on domain-specific audio (medical terms, technical jargon)
- Audio playback + transcript review in the UI
- Voice-history viewer

### Storage shape stays put
The rich `content: [<blocks>]` storage format already supports image / audio / video block types — code paths are stubbed in v0.0.1, this quarter wires them through. No data migration needed; your text-only datasets keep working unchanged.

---

## Q3 2027 — v0.4.x

**Theme: Beyond fine-tuning.** A solution today is just `(base_model, adapter)`. This quarter expands what a deployable solution can be.

### RAG solutions
- Vector index as a first-class component alongside the model adapter
- Knowledge datasets (already a type in the schema, currently inert) become embedding sources
- Hybrid search: dense vectors + BM25, configurable per solution
- Per-document permissions for multi-tenant deployments
- Citation surfacing in chat responses

### Agent solutions
- Tool definitions stored alongside the solution
- Built-in tools: web fetch (with allowlist), file read, code execution sandbox
- Custom tools via OpenAPI spec import
- Tool-call traces in chat history

### Deployment improvements
- vLLM runtime as production alternative to FastAPI (10-50× throughput)
- Model quantization (GPTQ, AWQ) at deploy time
- A/B routing between solutions on the same endpoint

---

## Q4 2027 — v0.5.x

**Theme: Production hardening.** Everything above gets battle-tested for the audit-heavy verticals (finance, defense) Spinalis is built for.

### Compliance and audit
- Tamper-evident audit log for every training and inference event
- Configurable retention policies for chat history and corrections
- SOC 2-friendly export of all activity for a given time window

### Multi-user
- User accounts with role-based access (admin / editor / viewer)
- Project-scoped permissions
- API tokens with per-token rate limits
- LDAP / SAML SSO for enterprise installs

### Multi-machine
- Compute Provider abstraction expanded: schedule training jobs across a pool of GPU servers
- Distributed inference for solutions that don't fit on one GPU
- Cluster-aware license enforcement

---

## H1 2028 and beyond — v1.0+

**Theme: Stability commitment.**

The 1.0 release marks the API and config-format compatibility commitment — once we hit it, breaking changes go through deprecation cycles, not surprise upgrades. Specifically:

- Storage format frozen (rich `content` block shape)
- REST API frozen at `/api/v1/`
- Build artifact format frozen (adapters from v1.0 always loadable in later versions)
- Config schema frozen (new optional fields fine; renames or removals require v2.0)

After v1.0 the public roadmap shifts to a quarterly cycle with explicit RFCs for major features. For now, the items above are the active backlog.

---

## Things explicitly **not** on the roadmap

To keep the scope honest:

- **Hosted SaaS version.** Spinalis's identity is on-premise. Customers who want hosted have a hundred options.
- **Pre-training.** The platform is for fine-tuning existing base models, not training from scratch. Training from scratch needs different infrastructure (multi-node clusters, weeks of compute) that doesn't fit the "single-GPU desktop or single-server" target.
- **Web search built-in.** Tool support (Q3 2027) lets you wire it up; we don't ship it.
- **Reinforcement learning beyond DPO/GRPO/ORPO/KTO.** PPO with a separate reward model is in scope. Anything more exotic is out.

---

## Want to influence the order?

The roadmap above is shaped by what design partners need. If you have a use case that isn't covered, say so:

- Open a feature request: [GitHub Issues](https://github.com/Basilisk-Inc/spinalis-releases/issues/new?template=feature_request.yml)
- Talk it through: [book a 30-min call](TODO_CALENDLY_LINK)
- Drop into the chat: [Discord](TODO_DISCORD_INVITE) / [Slack](TODO_SLACK_INVITE) / [Telegram](TODO_TELEGRAM_LINK)

Enterprise customers with paid contracts have direct influence on prioritization — that's part of the deal.