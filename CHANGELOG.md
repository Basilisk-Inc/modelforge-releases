# Changelog

All notable changes to Spinalis will be documented here. The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and the project adheres to [Semantic Versioning](https://semver.org/) — though, until v1.0, breaking changes can land on minor bumps.

## [Unreleased]

Nothing here yet — open a [feature request](https://github.com/Basilisk-Inc/spinalis-releases/issues/new?template=feature_request.yml) if you want something prioritized.

## [0.0.1] — 2026-05-04

First public release. Production-quality core, intentionally narrow surface area.

### Added

#### Fine-tuning
- QLoRA trainer with 4-bit NF4 quantization (default method)
- LoRA trainer for bf16 fine-tuning when VRAM permits
- Pre-validated base models: Qwen2.5 (0.5B–7B), Phi-4-mini, Gemma 3 4B, Florence-2 base, Whisper-large-v3-turbo
- Manual model addition via Hugging Face URL
- Training configs: epochs, batch size, gradient accumulation, learning rate, warmup, weight decay, max sequence length, LoRA rank/alpha/dropout
- Per-step metrics (loss / grad-norm / token accuracy / learning rate) streamed to the UI

#### Datasets
- Storage format: `jsonl_sft` and `jsonl_dpo` with rich multi-modal-friendly `content: [<blocks>]` shape
- Auto-detection of legacy interchange formats: Alpaca, Q&A, prompt/response
- Dataset analyzer: schema, field statistics, text statistics (token counts, vocabulary, language distribution), quality report (duplicates, outliers, completeness), model-compatibility tags
- Train/eval/test splitting with optional stratified sampling
- One-click train+eval split with analyzer-recommended ratio
- Correction-target datasets — thumbs up/down in chat write training records to a fresh dataset
- Dataset preview (first N records) directly in the UI

#### Compute and serving
- Multi-GPU vendor support: NVIDIA (CUDA 12.1), AMD ROCm (HIP), Apple Silicon MPS, Windows AMD via DirectML
- Local subprocess compute provider with structured Go ↔ Python contract (env vars + JSONL metrics tail)
- Resource monitor: GPU/CPU/memory/disk/network sampled every 5 seconds, 24h history for graphs
- FastAPI inference runtime, OpenAI-compatible `/v1/chat/completions` endpoint
- Per-endpoint health monitoring with auto-status updates

#### Chat persistence
- Chat sessions stored in SQLite metadata + JSONL message bodies
- Optional `X-Spinalis-Session-Id` header on the inference proxy enables auto-persistence of API-driven conversations
- `persist_history` toggle on each session — opt-out for ephemeral chats
- Project-scoped chat history page with delete affordance
- Inline thumbs-up/down on historical messages — corrections flow into the same dataset pipeline as live chat

#### Licensing
- Per-GPU-per-machine licensing with one free GPU on every install
- Three tiers: Community (free), Pro, Enterprise
- Ed25519-signed license files
- Fuzzy 2-of-3 machine fingerprinting (CPU ID, motherboard serial, MAC) — single-component swaps don't invalidate
- Online activation via a license server
- Offline activation via fingerprint export → license import
- Anti-rollback timestamp protection (Linux HMAC-file, macOS Keychain, Windows DPAPI)

#### Installation and packaging
- Bundled Python 3.12.11 (python-build-standalone) for Linux/Windows/macOS — no system Python required
- One-click installers: Windows Inno Setup wizard, macOS .pkg with system Installer, Linux interactive `install.sh`
- Per-user vs per-machine install location on Windows
- Custom data directory selection on Windows wizard
- Garble obfuscation on release builds (defense-in-depth, not DRM)

#### Documentation
- Six in-app docs sections (Installation, Workflow, Datasets, Fine-tuning, API, Licensing)
- Self-contained Markdown renderer in the UI (no `react-markdown` dep)
- Code blocks with language label and copy button
- Same content available standalone in this repo's `docs/` folder

#### API
- REST + WebSocket under `/api/v1/`
- License tier exposed via response headers (`X-Spinalis-Tier`, `X-Spinalis-GPU-Budget`)
- Project-scoped session listing endpoint
- Correction submission with format-locked dataset targeting

### Known limitations

These aren't bugs — they're scope decisions for v0.0.1. See [`ROADMAP.md`](ROADMAP.md) for when each lifts.

- **DPO/GRPO trainers not yet implemented.** The `jsonl_dpo` storage format is in place; the trainer ships in v0.1.x.
- **Vision and audio fine-tuning not yet wired through.** Florence-2 and Whisper are present as base models for inference; fine-tuning lands in v0.2.x.
- **No multi-user accounts.** Single-operator on each installation. Multi-tenancy is v0.4.x.
- **Installers are unsigned.** Windows SmartScreen and macOS Gatekeeper will warn on first launch. Code signing comes with v0.4.x.
- **No vLLM runtime.** Production-throughput serving lands in v0.3.x; FastAPI is fine for development and small deployments.
- **No tools / RAG.** Solutions are model-only today. RAG and agent solutions are v0.3.x.

---

[Unreleased]: https://github.com/Basilisk-Inc/spinalis-releases/compare/v0.0.1...HEAD
[0.0.1]: https://github.com/Basilisk-Inc/spinalis-releases/releases/tag/v0.0.1