# ModelForge

> **On-premise fine-tuning platform for small language models.** Bring your data, fine-tune Qwen / Phi / Gemma on your own GPUs, deploy behind an OpenAI-compatible API. Your data never leaves your network.

[![Latest release](https://img.shields.io/github/v/release/TODO_GITHUB_ORG/modelforge-releases?label=release)](https://github.com/TODO_GITHUB_ORG/modelforge-releases/releases/latest)
[![Discord](https://img.shields.io/badge/Discord-join-5865F2?logo=discord&logoColor=white)](TODO_DISCORD_INVITE)
[![License](https://img.shields.io/badge/license-Proprietary-blue.svg)](LICENSE.md)

ModelForge runs entirely on your infrastructure — desktop with a single GPU, on-prem server, or air-gapped environment. No data ever leaves the machine. Fine-tune small language models on your private datasets, then serve them through an OpenAI-compatible endpoint that your existing tooling already speaks.

---

## Why ModelForge

You probably can't send your data to OpenAI / Anthropic / Hugging Face. Maybe it's regulated (HIPAA, GDPR, financial records). Maybe it's classified. Maybe your CISO simply doesn't want a third party touching it. The cloud-AI playbook stops working at that boundary.

The DIY alternative — wiring together transformers, peft, trl, vllm, your own license server, your own monitoring — is a six-month project before the first model trains. ModelForge is that stack, packaged.

- **Data sovereignty by design.** No telemetry. No phone-home. Air-gapped installs are a first-class case, not an afterthought.
- **One install, four platforms.** Linux, Windows, macOS Intel, macOS Apple Silicon. Bundled Python — no system Python required.
- **Multi-GPU vendor support.** NVIDIA (CUDA), AMD (ROCm), Apple (MPS), Windows AMD (DirectML).
- **OpenAI-compatible serving.** Existing clients work unchanged. Drop-in replacement for the cloud API in test environments.
- **Reflective workflow.** Conversations recorded → corrections marked → corrections become training data → next fine-tune is better. The loop closes on itself.

---

## Quick start

### 1. Download

Grab the build for your platform from the [Releases page](https://github.com/TODO_GITHUB_ORG/modelforge-releases/releases/latest).

| Platform | File | Format |
|---|---|---|
| Linux x86_64 | `modelforge-linux-amd64-v0.0.1.tar.gz` | tarball + `install.sh` |
| Windows x86_64 | `modelforge-windows-amd64-v0.0.1-setup.exe` | Inno Setup wizard |
| macOS Apple Silicon | `ModelForge-v0.0.1-arm64.pkg` | macOS Installer |
| macOS Intel | `ModelForge-v0.0.1-amd64.pkg` | macOS Installer |

### 2. Install

**Linux:**
```bash
tar xzf modelforge-linux-amd64-v0.0.1.tar.gz
cd modelforge-linux-amd64-v0.0.1
./install.sh
```

**Windows:** double-click `modelforge-windows-amd64-v0.0.1-setup.exe`. Wizard walks you through it.

**macOS:** double-click `.pkg`. The system Installer handles the rest.

### 3. Launch

```bash
modelforge serve
```

UI opens at <http://localhost:8080>. Create your first project, upload a dataset, run a fine-tune.

The first GPU is **free forever** — no signup, no license required. Need more GPUs on one machine? See [Licensing](#licensing) below.

---

## How it works

```
Project → Dataset → Build → Solution → Endpoint
   │         │        │         │          │
   │         │        │         │          └─ Running OpenAI-compatible inference
   │         │        │         └─ Deployable artifact (model + adapter + config)
   │         │        └─ Fine-tuning + evaluation run
   │         └─ JSONL/CSV in any of the common formats — auto-analyzed
   └─ Logical container, scoped to one use case
```

A complete walkthrough lives in the **Workflow** doc inside the app — open Sidebar → Docs after install. The same content is also in this repo under [`docs/`](docs/).

---

## Supported models

Pre-validated and tested:

| Family | Sizes | Use case |
|---|---|---|
| **Qwen 2.5** | 0.5B / 1.5B / 3B / 7B | General-purpose text, default starter |
| **Phi-4** | 3.8B | Reasoning + code |
| **Gemma 3** | 4B | Multilingual |
| **Florence-2** | 0.27B | Vision (caption / detect / OCR) |
| **Whisper** | 0.8B | Audio transcription |

Any other Hugging Face model can be added at runtime — paste an `org/repo` URL, ModelForge fetches the card and downloads it.

Fine-tuning methods: **QLoRA** (4-bit, default), **LoRA** (bf16, when you have the VRAM). DPO and GRPO are on the roadmap — see [`ROADMAP.md`](ROADMAP.md).

---

## Hardware requirements

| Resource | Minimum | Recommended |
|---|---|---|
| RAM | 16 GB | 32 GB |
| Disk | 50 GB free | 200 GB+ |
| GPU | None (CPU works for inference) | NVIDIA 8+ GB VRAM, AMD 16+ GB, or Apple Silicon |
| OS | Ubuntu 22.04, Windows 10, macOS 13 | Ubuntu 24.04, Windows 11, macOS 14+ |
| Network | Only for first-run model download | — |

QLoRA on Qwen2.5-1.5B fits in **4 GB VRAM**. The 7B model needs ~10 GB. Fine-tuning a 7B model on a single 3090 (24 GB) takes 3-5 minutes on a thousand records.

---

## Licensing

ModelForge is licensed **per GPU per machine**. The first GPU is free.

| Tier | GPUs | Cost | Best for |
|---|---|---|---|
| **Community** | 1 | Free | Single-GPU desktop, evaluation, hobbyists |
| **Pro** | up to 4 / node | ~$149 / GPU / month | Production deployments |
| **Enterprise** | unlimited | Custom (min commit) | Multi-server / multi-team |

Online and **offline activation** are both supported — the offline flow exports a fingerprint file, you mail it (or upload via a customer portal), and a signed license file comes back. No persistent network connection ever required.

For commercial licensing inquiries: TODO_SALES_EMAIL or [book a call](TODO_CALENDLY_LINK).

Full EULA: [`LICENSE.md`](LICENSE.md).

---

## What's next

Active development. Highlights of the next two quarters:

- **DPO + GRPO** preference fine-tuning
- **Vision fine-tuning** (Florence-2, LLaVA-style models)
- **Audio transcription** with Whisper
- **RAG solutions** with built-in vector indexes
- **Agent solutions** — tool-using endpoints

Full breakdown by quarter: [`ROADMAP.md`](ROADMAP.md).

---

## Get in touch

- **Bug reports / specific feature requests** → [GitHub Issues](https://github.com/TODO_GITHUB_ORG/modelforge-releases/issues/new/choose)
- **Questions, discussion, "is X possible"** → community chat:
    - Discord: TODO_DISCORD_INVITE
    - Slack: TODO_SLACK_INVITE
    - Telegram: TODO_TELEGRAM_LINK
- **Demos / commercial / enterprise** → [book a 30-min call](TODO_CALENDLY_LINK)
- **Security** — please use the private channel in [`SECURITY.md`](SECURITY.md), not public issues.

---

## Documentation

In-app: open the running ModelForge UI → Sidebar → **Docs**. All six sections also live in this repo under [`docs/`](docs/) for reading without an install:

- [Installation](docs/01-installation.md)
- [Workflow](docs/02-workflow.md)
- [Datasets](docs/03-datasets.md) — supported formats, corrections
- [Fine-tuning](docs/04-fine-tuning.md) — QLoRA/LoRA configs
- [API reference](docs/05-api.md) — chat completions, persistence
- [Licensing](docs/06-licensing.md) — activation, fingerprinting

---

## Status

**v0.0.1** — first public release. Production-quality core (fine-tuning, evaluation, serving, license enforcement) but the surface area is intentionally narrow. Expect breaking config changes through v0.x; we'll commit to backward compatibility starting v1.0.

See [`CHANGELOG.md`](CHANGELOG.md) for what landed in this release. We follow [Keep a Changelog](https://keepachangelog.com/) and [Semantic Versioning](https://semver.org/).