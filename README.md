<p align="center">
  <img src="assets/spinalis-transparent.png" alt="Spinalis" width="160"/>
</p>

<h1 align="center">Spinalis</h1>

<p align="center">
  <strong>On-premise fine-tuning platform for small language models.</strong><br/>
  Bring your data, fine-tune Qwen / Phi / Gemma on your own GPUs, deploy behind an OpenAI-compatible API. Your data never leaves your network.
</p>

<p align="center">
  <a href="https://github.com/Basilisk-Inc/spinalis-releases/releases/latest"><img src="https://img.shields.io/github/v/release/Basilisk-Inc/spinalis-releases?label=release" alt="Latest release"/></a>
  <a href="TODO_DISCORD_INVITE"><img src="https://img.shields.io/badge/Discord-join-5865F2?logo=discord&logoColor=white" alt="Discord"/></a>
  <a href="LICENSE.md"><img src="https://img.shields.io/badge/license-Proprietary-blue.svg" alt="License"/></a>
</p>

---

Spinalis runs entirely on your infrastructure — desktop with a single GPU, on-prem server, or air-gapped environment. No data ever leaves the machine. Fine-tune small language models on your private datasets, then serve them through an OpenAI-compatible endpoint that your existing tooling already speaks.

> **v0.0.1 — first public preview.** Core fine-tuning, evaluation, serving, and license enforcement are production-quality. The release surface is intentionally narrow: NVIDIA-first, CLI installers on Windows/Linux, partial Apple Silicon. See [Status](#status) for what's solid vs. what's coming.

---

## Why Spinalis

You probably can't send your data to OpenAI / Anthropic / Hugging Face. Maybe it's regulated (HIPAA, GDPR, financial records). Maybe it's classified. Maybe your CISO simply doesn't want a third party touching it. The cloud-AI playbook stops working at that boundary.

The DIY alternative — wiring together transformers, peft, trl, vllm, your own license server, your own monitoring — is a six-month project before the first model trains. Spinalis is that stack, packaged.

- **Data sovereignty by design.** No telemetry. No phone-home. Air-gapped installs are a first-class case, not an afterthought.
- **One install, four platforms.** Linux, Windows, macOS Intel, macOS Apple Silicon. Bundled Python 3.12 — no system Python required.
- **OpenAI-compatible serving.** Existing clients work unchanged. Drop-in replacement for the cloud API in test environments.
- **Reflective workflow.** Conversations recorded → corrections marked → corrections become training data → next fine-tune is better. The loop closes on itself.

---

## Quick start

### 1. Download

Grab the build for your platform from the [Releases page](https://github.com/Basilisk-Inc/spinalis-releases/releases/latest).

| Platform | File | What you get |
|---|---|---|
| Linux x86_64 | `spinalis-linux-amd64-v0.0.1.tar.gz` | tarball + `install.sh` |
| Windows x86_64 | `spinalis-windows-amd64-v0.0.1.zip` | zip + `install.ps1` |
| macOS Apple Silicon | `spinalis-darwin-arm64-v0.0.1.tar.gz` | tarball + `install.sh` |
| macOS Intel | `spinalis-darwin-amd64-v0.0.1.tar.gz` | tarball + `install.sh` |

Double-clickable installers (Inno Setup `.exe` on Windows, `.pkg` on macOS) are in the next release. See [Roadmap → v0.1](#whats-next).

### 2. Install

**Linux:**
```bash
mkdir spinalis && cd spinalis
tar xzf ../spinalis-linux-amd64-v0.0.1.tar.gz
./install.sh
```

**macOS:**
```bash
mkdir spinalis && cd spinalis
tar xzf ../spinalis-darwin-arm64-v0.0.1.tar.gz   # or -amd64 for Intel
./install.sh
```

**Windows** — please read [the antivirus note](#windows-defender--smartscreen) below first. Then:

```powershell
# In an Administrator PowerShell, BEFORE extracting:
Add-MpPreference -ExclusionPath "$env:USERPROFILE\Downloads"
Add-MpPreference -ExclusionPath "$env:LOCALAPPDATA\Spinalis"

# Extract the zip (right-click → Extract All), then in a normal PowerShell:
cd "<extracted folder>"
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\install.ps1
```

The installer asks whether to use the **bundled** Python 3.12 (recommended, isolated) or your **system** Python if you have one. Either way it creates a private venv and pulls PyTorch + the ML stack into it — your existing Python environments are not modified.

The full install transcript is saved to `install.log` inside the install directory. Keep it if you need to file an issue.

### 3. Launch

```bash
spinalis        # Linux/macOS
spinalis        # Windows, in a new terminal after install
```

UI opens at <http://localhost:8080>. Create your first project, upload a dataset, run a fine-tune.

The first GPU is **free forever** — no signup, no license required. Need more GPUs on one machine? See [Licensing](#licensing).

---

### Windows Defender / SmartScreen

The current Windows binary is **unsigned** (an EV code-signing certificate is on the roadmap for v0.1) and obfuscated with [`garble`](https://github.com/burrowers/garble) to protect license-enforcement code. Both characteristics make Windows Defender's heuristic scanner nervous, and on a meaningful fraction of machines you'll see one of:

- **SmartScreen warning** at launch — click *More info → Run anyway*.
- **Defender quarantines `spinalis.exe` silently during extraction** — the file simply isn't there after you unzip. This is the more annoying case.

Why this happens and what to do:

1. **Add an exclusion BEFORE downloading**, not after. Once Defender quarantines the file, even re-downloading won't help until the exclusion is in place.
   ```powershell
   Add-MpPreference -ExclusionPath "$env:USERPROFILE\Downloads"
   Add-MpPreference -ExclusionPath "$env:LOCALAPPDATA\Spinalis"
   ```
2. **Verify `spinalis.exe` is in the extracted folder** before running `install.ps1`. If it's missing, the exclusion didn't apply — confirm with `Get-MpPreference | Select-Object -ExpandProperty ExclusionPath`, then re-extract.
3. We submit every release SHA-256 to [Microsoft's false-positive review form](https://www.microsoft.com/en-us/wdsi/filesubmission). Detection rates drop over the first 1-2 weeks of each release as their reputation database catches up.

This is a known unsigned-binary problem, not a Spinalis bug. We're acquiring an EV cert; once it's in place this section becomes obsolete.

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

A complete walkthrough lives in the **Workflow** doc inside the app — open Sidebar → Docs after install. The same content is in this repo under [`docs/`](docs/).

---

## Supported models

Pre-validated and tested:

| Family | Sizes | Use case |
|---|---|---|
| **Qwen 2.5** | 0.5B / 1.5B / 3B / 7B | General-purpose text, default starter |
| **Phi-3.5 / Phi-4** | 3.8B | Reasoning + code |
| **Gemma 2 / 3** | 2B / 4B | Multilingual |
| **Florence-2** | 0.27B | Vision (caption / detect / OCR) |
| **Whisper** | small / medium | Audio transcription |

Any other Hugging Face model can be added at runtime — paste an `org/repo` URL, Spinalis fetches the card and downloads the weights.

Fine-tuning methods: **QLoRA** (4-bit, default), **LoRA** (bf16, when you have the VRAM). DPO and GRPO are on the roadmap — see [`ROADMAP.md`](ROADMAP.md).

---

## GPU support

Spinalis runs on NVIDIA today. Apple Silicon training works through PyTorch's MPS backend but is partially integrated. AMD and DirectML are on the roadmap.

| Vendor | Detected in UI | Training | Inference |
|---|---|---|---|
| **NVIDIA** (driver 535+, CUDA 12.1) | ✅ | ✅ | ✅ |
| **Apple Silicon** (M1+, macOS 13+) | ⚠️ shows "CPU-only" | ✅ via MPS | ✅ via MPS |
| **AMD (ROCm)** | ❌ | ❌ | ❌ — v0.2 |
| **Windows AMD (DirectML)** | ❌ | ❌ | ❌ — v0.2 |
| **CPU only** | ✅ | ⚠️ very slow | ✅ slow |

**Apple Silicon note.** The UI currently displays "CPU-only" on Macs because the server's GPU discovery only knows about `nvidia-smi`. The Python runtime correctly uses MPS for training and inference — only the resource page label is wrong. Fix in v0.1.

**NVIDIA + Linux Secure Boot.** If `nvidia-smi` exits with "couldn't communicate with the NVIDIA driver" but the driver package is installed, the kernel module is probably blocked by Secure Boot. Enroll it via `mokutil --import /var/lib/shim-signed/mok/MOK.der` and reboot to the blue MOK Manager screen — there's a full walkthrough in [`docs/01-installation.md`](docs/01-installation.md).

---

## Hardware requirements

|  | Inference only | Fine-tuning (≤3B) | Fine-tuning (7B+) |
|---|---|---|---|
| RAM | 16 GB | 32 GB | 64 GB |
| Disk free | 50 GB | 100 GB | 200+ GB |
| GPU | Optional | NVIDIA 8 GB VRAM, or Apple Silicon | NVIDIA 16 GB VRAM (QLoRA 4-bit) |
| OS | Ubuntu 22.04+, Windows 10 1803+, macOS 13+ | Same | Same |
| Network | First-run model download only | Same | Same |

QLoRA on Qwen 2.5-1.5B fits in ~4 GB VRAM. The 7B model needs ~10 GB with 4-bit quantization. Fine-tuning a 7B model on a single RTX 3090 (24 GB) takes 3-5 minutes on a thousand records.

---

## Licensing

Spinalis is licensed **per GPU per machine**. The first GPU is free.

| Tier | GPUs | Cost | Best for |
|---|---|---|---|
| **Community** | 1 | Free | Single-GPU desktop, evaluation, hobbyists |
| **Pro** | up to 4 / node | ~$149 / GPU / month | Production deployments |
| **Enterprise** | unlimited | Custom (min commit) | Multi-server / multi-team |

**Online and offline activation** are both supported. The offline flow exports a fingerprint file, you mail it (or upload via a customer portal), and a signed license file comes back. No persistent network connection ever required.

For commercial licensing inquiries: **TODO_SALES_EMAIL** or [book a call](TODO_CALENDLY_LINK).

Full EULA: [`LICENSE.md`](LICENSE.md).

---

## What's next

Active development by a solo founder — dates in [`ROADMAP.md`](ROADMAP.md) shift when reality demands it.

**Coming in v0.1 (Q4 2026)**
- **Inno Setup wizard** for Windows + **macOS `.pkg`** — double-clickable installers
- **EV code signing** (Windows) + **Apple notarization** (macOS) — no more SmartScreen / Gatekeeper warnings
- **Apple Silicon MPS** correctly surfaced in the UI Resources page
- **DPO preference fine-tuning** with the existing thumbs-down correction flow
- **Diagnostic endpoint** for one-command "what's wrong with my install?" reports

**Later**
- AMD ROCm support (Q1 2027)
- DirectML for Windows AMD (Q1 2027)
- GRPO reasoning training (Q1 2027)
- Vision / audio fine-tuning (Q2 2027)
- RAG and agent solutions (Q3 2027)
- Compliance, audit log, multi-user, multi-machine (Q4 2027)

Full breakdown: [`ROADMAP.md`](ROADMAP.md).

---

## Get in touch

- **Bug reports / specific feature requests** → [GitHub Issues](https://github.com/Basilisk-Inc/spinalis-releases/issues/new/choose). Please attach `install.log` (from your install directory) and the server's startup output — the JSON-formatted lines at the top.
- **Questions, discussion, "is X possible"** → community chat:
    - Discord: TODO_DISCORD_INVITE
    - Slack: TODO_SLACK_INVITE
    - Telegram: TODO_TELEGRAM_LINK
- **Email** for licensing, air-gapped deployment, or anything not suitable for public discussion: **TODO_SUPPORT_EMAIL**
- **Demos / commercial / enterprise** → [book a 30-min call](TODO_CALENDLY_LINK)
- **Security** — please use the private channel in [`SECURITY.md`](SECURITY.md), not public issues.

We respond fastest to bug reports that include:

- OS + architecture
- GPU + driver version (output of `nvidia-smi` if NVIDIA)
- Whether you chose bundled or system Python during install
- The actual error message, not "it didn't work"
- The install log if it's an install issue, the server startup log if it's a runtime issue

---

## Documentation

In-app: open the running Spinalis UI → Sidebar → **Docs**. All sections also live in this repo under [`docs/`](docs/) for reading without an install:

- [Installation](docs/01-installation.md) — Linux / Windows / macOS, including the Defender workaround and Secure Boot for NVIDIA
- [Workflow](docs/02-workflow.md) — project → dataset → build → solution → endpoint
- [Datasets](docs/03-datasets.md) — supported formats, splits, corrections
- [Fine-tuning](docs/04-fine-tuning.md) — QLoRA / LoRA configs and what to change
- [API reference](docs/05-api.md) — chat completions, persistence, the `X-Spinalis-Session-Id` header
- [Licensing](docs/06-licensing.md) — activation, fingerprinting, offline flow

---

## Status

**v0.0.1 — first public release.** The core (fine-tuning, evaluation, serving, license enforcement) is production-quality. The surface area is intentionally narrow:

**Works today**
- NVIDIA training & inference on Linux and Windows
- QLoRA / LoRA on Qwen, Phi, Gemma; vision on Florence-2; audio on Whisper
- Per-GPU licensing with online and offline activation
- OpenAI-compatible chat completions with optional session persistence
- Conversation recording, message-level corrections, and export of corrections as SFT/DPO datasets
- CLI installers on all four platforms

**Coming in v0.1 (Q4 2026)**
- Double-clickable installers (Inno Setup, macOS `.pkg`)
- EV code signing → no more SmartScreen / Defender warnings
- Apple Silicon MPS detection in the UI
- DPO preference fine-tuning
- Diagnostic endpoint

Expect breaking config changes through v0.x; we'll commit to backward compatibility starting v1.0.

See [`CHANGELOG.md`](CHANGELOG.md) for what landed in this release. We follow [Keep a Changelog](https://keepachangelog.com/) and [Semantic Versioning](https://semver.org/).