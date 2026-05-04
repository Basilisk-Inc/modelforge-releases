# Roadmap

Where ModelForge is going. Dates are intent, not promises — solo development means scope shifts when reality demands it. This document is updated with every release.

For "what's already shipped," see [`CHANGELOG.md`](CHANGELOG.md).

---

## Now (Q2 2026, v0.0.x)

The shipped foundation. Documented here so you know where the line between "works today" and "coming soon" sits.

- ✅ Fine-tuning: QLoRA, LoRA on text models
- ✅ Supported models: Qwen 2.5 (0.5B–7B), Phi-4-mini, Gemma 3, Florence-2, Whisper-large-v3-turbo
- ✅ Custom model addition via Hugging Face URL
- ✅ Multi-GPU vendor support: NVIDIA / AMD ROCm / Apple MPS / Windows DirectML
- ✅ OpenAI-compatible serving (FastAPI runtime)
- ✅ Dataset analyzer: schema, statistics, quality issues, model-compatibility tags
- ✅ Train/eval split with stratified sampling
- ✅ Chat playground with streaming responses + reasoning controls
- ✅ Chat history persistence (in-app + via API header)
- ✅ Correction-driven datasets — thumbs up/down → next training round
- ✅ Per-GPU licensing with offline activation
- ✅ One-click installers: Linux tarball, Windows Inno Setup, macOS .pkg
- ✅ In-app documentation

---

## Q3 2026 (v0.1.x)

**Theme: Beyond SFT.** The basics work; this quarter expands what kinds of training you can do.

### DPO and preference fine-tuning
- DPO trainer with the existing `jsonl_dpo` storage format already in place
- Correction flow extended: thumbs-down → "what should it have said" → DPO record
- Reference model selection (frozen base vs different SFT adapter)

### GRPO and reasoning training
- GRPO trainer for reasoning-heavy tasks (math, code, multi-step)
- `train_on_reasoning: true` already in the config — wire to a proper reasoning-aware loss
- Reasoning-trace dataset templates for distilling R1-style models into smaller targets

### Evaluation harness
- BLEU / ROUGE / exact-match for SFT
- Win-rate eval for DPO (paired-comparison against base model)
- Per-record annotated outputs viewable in the UI

### Better dataset tooling
- Dataset deduplication (currently only counts duplicates; next iteration removes them)
- Dataset diff view — what changed between v1 and v2 of the same data
- Bulk correction import from CSV (for teams already labeling outside ModelForge)

---

## Q4 2026 (v0.2.x)

**Theme: Multi-modal.** Text-only is the cheap part. This quarter brings vision and audio fine-tuning end to end.

### Vision fine-tuning
- LLaVA-1.5 / 1.6 architecture support
- Florence-2 fine-tuning (caption, detect, OCR head retraining)
- Image-grounded chat in the playground (drag-drop image → multi-modal query)
- Image-history viewer — vision equivalent of today's chat history page

### Audio transcription
- Whisper fine-tuning on domain-specific audio (medical terms, technical jargon)
- Audio playback + transcript review in the UI
- Voice-history viewer

### Storage shape stays put
The rich `content: [<blocks>]` storage format already supports image / audio / video block types — code paths are stubbed, this quarter wires them through. No data migration needed; your text-only datasets keep working unchanged.

---

## Q1 2027 (v0.3.x)

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

## Q2 2027 (v0.4.x)

**Theme: Production hardening.** Everything above gets battle-tested for the audit-heavy verticals (finance, defense) ModelForge is built for.

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

### Code signing for installers
- Windows EV cert (production releases stop showing SmartScreen warnings)
- macOS Developer ID + notarization (no more "right-click → Open" dance)

---

## H2 2027 and beyond (v1.0+)

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

- **Hosted SaaS version.** ModelForge's identity is on-premise. Customers who want hosted have a hundred options.
- **Pre-training.** The platform is for fine-tuning existing base models, not training from scratch. Training from scratch needs different infrastructure (multi-node clusters, weeks of compute) that doesn't fit the "single-GPU desktop or single-server" target.
- **Web search built-in.** Tool support (Q1 2027) lets you wire it up; we don't ship it.
- **Reinforcement learning beyond DPO/GRPO/ORPO/KTO.** PPO with a separate reward model is in scope. Anything more exotic is out.

---

## Want to influence the order?

The roadmap above is shaped by what design partners need. If you have a use case that isn't covered, say so:

- Open a feature request: [GitHub Issues](https://github.com/TODO_GITHUB_ORG/modelforge-releases/issues/new?template=feature_request.yml)
- Talk it through: [book a 30-min call](TODO_CALENDLY_LINK)
- Drop into the chat: [Discord](TODO_DISCORD_INVITE) / [Slack](TODO_SLACK_INVITE) / [Telegram](TODO_TELEGRAM_LINK)

Enterprise customers with paid contracts have direct influence on prioritization — that's part of the deal.