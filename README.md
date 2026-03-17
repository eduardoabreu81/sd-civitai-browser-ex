<div align="center">
  <img src=".github/logo.png?v=2" alt="CivitAI Browser Neo"/>
</div>

# 🎨 CivitAI Browser EX

<div align="center">

[![Forge Classic / A1111](https://img.shields.io/badge/Gradio-3.41.2-orange)](https://gradio.app/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

> **Extension for [A1111](https://github.com/AUTOMATIC1111/stable-diffusion-webui) and [Forge Classic](https://github.com/Haoming02/sd-webui-forge-classic/)**

</div>

Browse, download, and manage your CivitAI models directly inside the WebUI — with auto-organization, disk usage dashboard, creator management, and support for all modern architectures (FLUX, Wan, Qwen, Pony, Illustrious, and more).

---

## 📋 Table of Contents

- [What's New](#-whats-new)
- [Changelog](#-changelog)
- [Roadmap](#️-roadmap)
- [Features](#-features)
- [Installation](#-installation)
- [Auto-Organization System](#-auto-organization-system)
- [Dashboard & Statistics](#-dashboard--statistics)
- [Supported Model Types](#-supported-model-types)
- [Credits](#-credits)

---

## 🆕 What's New

### v0.8.2-ex — Checkpoint SHA256 Cache Sync

- **Automatic cache sync after checkpoint download** — when a checkpoint download finishes successfully, its SHA256 is now written to WebUI cache immediately.
- **Manual sync button in Update Models** — new action `Sync checkpoint SHA256 cache` scans local checkpoints and adds only missing SHA256 entries to WebUI cache.
- **Local checkpoint hash registry** — new local registry tracks synced checkpoints and prunes stale entries when files are deleted from disk.

---

## 📖 Changelog

### v0.8.2-ex — Checkpoint SHA256 Cache Sync
- Added automatic SHA256 cache sync for checkpoints right after successful download completion.
- Added a manual `Sync checkpoint SHA256 cache` button in Update Models to reconcile local checkpoints against WebUI cache.
- Added local checkpoint hash registry (`lib/models/checkpoint_hashes.json`) to track synced entries and clean stale records for deleted files.

### v0.8.1-ex — Trigger Word Bugfixes & Resilience
- Fixed an issue where the local trigger word fallback process ignored API groups and flattened words into single lines.
- Fixed an issue where "Update model info & tags" didn't safely persist incoming `trainedWords` groups natively to the local cache.
- Added exponential backoff retry mechanism to API calls returning temporary 50x server errors, saving "Update models" loops from failing silently on affected files.
- Moved trigger word row buttons (📋 / ➕) to the left side of the text in the preview panel.

### v0.8.0-ex — Trigger Word Consolidation
- Consolidated trigger words from `.safetensors` metadata, local `.json` `activation text`, and API `trainedWords`
- Added case-insensitive deduplication while preserving original order
- Model info now uses local consolidated trigger words first, with API fallback when local cache is unavailable

### v0.7.4-ex — Wan I2V/T2V Differentiation
- Wan card badges now distinguish `I2V`, `T2V`, and `TI2V` subtypes (API already returns specific `baseModel` values)
- New setting `civitai_neo_wan_subfolder_by_type` (OFF by default): splits Wan downloads into `Wan/I2V/`, `Wan/T2V/`, `Wan/TI2V/` subfolders
- Fixed multi-level subfolder "already organized" check (was using `os.path.basename`, broke for `Wan/I2V` paths)
- Fixed Flux.2 Klein 4B/9B and Flux.2 D showing `F1` badge — now correctly shows `F2`

### v0.7.3-ex — Per-group Trigger Word Rows
- Each trigger word group gets its own row with individual copy and add-to-prompt buttons
- LORA tag row (`<lora:filename:1>`) shown as first entry in purple/monospace
- Clipboard copy with ✓ visual feedback (1.5s)
- "Add all to prompt" button when multiple groups exist

### v0.7.2-ex — Bug Fixes
- Fixed wildcard base-model subfolder being applied in GUI even when `wildcard_organize_by_base` is OFF (`civitai_api.py` path calculation was missing the wildcard guard)
- Fixed delete-by-SHA256 silently failing — `json_base` path not joined with `root`, making `os.path.exists` search in CWD instead of the model directory

### v0.7.1-ex — Wildcard Download Improvements
- Own subfolder per wildcard download (sd-dynamic-prompts compatible)
- Flat zip extraction — no double-nesting when the zip has internal folders
- Skip preview/gallery images for Wildcards
- New settings: `wildcard_own_folder` (ON by default), `wildcard_organize_by_base` (OFF by default)

### v0.7.0-ex — Baseline
- Initial Release

---

## 🗺️ Roadmap

### v0.7.0-ex — Forge Classic/A1111 Compatibility *(complete)* ✅

### v0.7.1-ex — Wildcard Download Improvements *(complete)* ✅

### v0.7.2-ex — Bug Fixes *(complete)* ✅

### v0.7.3-ex — Per-group Trigger Word Rows *(complete)* ✅

### v0.7.4-ex — Wan I2V/T2V Differentiation *(complete)* ✅

### v0.8.0-ex — Trigger Word Consolidation *(complete)* ✅

### v0.8.1-ex — Trigger Word Bugfixes & Resilience *(complete)* ✅

### v0.8.2-ex — Checkpoint SHA256 Cache Sync *(complete)* ✅

### v0.9.0-ex — Advanced Curation *(planned)*
- Saved search presets
- Favorites in creator/user search
- Additional browser quality-of-life improvements
- **Organization by Tag — Phase 1**: save CivitAI tags to `.json` sidecar; editable user-tags field in model panel for manual assignment
- **Organization by Tag — Phase 2**: in Manage tab, pick "anchor" tags → models with that tag sort into `<type>/<tag>/` subfolders (independent of base-model organization)

### v1.0.0-ex — First Stable Release *(planned)*
- All known issues resolved
- Full A1111/Forge Classic compatibility guarantee

---

## 🎯 Features

> ⭐ = exclusive to Neo

### 🔍 Browse & Search

- Browse CivitAI directly inside the WebUI — no tab switching
- Search by model name, tag, or username
- Filter by content type: Checkpoint, LORA, VAE, ControlNet, Upscaler, TextualInversion, Wildcards, Workflows, and more
- Filter by base model: SD 1.x, SDXL, Pony, Illustrious, FLUX, Wan, Qwen, NoobAI, Lumina, and more — list auto-updated from CivitAI at startup ⭐
- Sort by: Highest Rated, Most Downloaded, Newest, Most Liked, Most Discussed
- Filter by time period: Day, Week, Month, Year, All Time
- NSFW toggle, liked-only filter, hide installed models, hide banned creators
- Exact search mode
- Search settings persist across restarts ⭐

### 📥 Download

- Download any model, version, and file variant directly
- High-speed multi-connection downloads via Aria2 (optional, on by default)
- Download queue — multiple downloads run in sequence without blocking the UI
- Queue persistence — survives session disconnects with one-click restore ⭐
- Cancel individually or clear the entire queue
- Folder automatically set based on content type ⭐
- Custom sub-folders per download
- API key support for early access and private models
- Proxy support for restricted regions

### 🔄 Model Updates ⭐

- Orange border on cards with a newer version available
- Batch update — select multiple outdated models and download all at once
- Version comparison by model family (not just version name)
- Retention policy on update: keep, move to trash, or replace
- Dashboard shows outdated model counts after scanning

### 🗂️ Auto-Organization ⭐

- New downloads automatically sorted into subfolders by base model (SDXL/, Pony/, FLUX/, etc.)
- Organize your existing collection in one click
- Validate organization — read-only check showing correct / misplaced / no-metadata per file ⭐
- Fix misplaced files in one click — automatic backup created first ⭐
- One-click rollback (keeps last 5 backups)
- Custom folder mapping in Settings
- Associated files (`.json`, `.png`, `.txt`) always move with the model

### 🖼️ Model Info & Preview

- Model info panel with name, version, base model, type, tags, permissions, and description
- Sample images with "Send to txt2img" — fills prompt, negative, sampler, steps, CFG
- Individual meta field buttons — send just one field; Shift+click to append ⭐
- "➕ Add to prompt" in the model overlay — appends trigger words directly; auto-inserts LoRA syntax ⭐
- SHA256 hash shown in version info — click to select ⭐
- Video preview on hover for cards with video samples ⭐
- Save model info and images locally

### 📊 Dashboard ⭐

- Disk usage by category and architecture
- Pie chart with percentage breakdown
- Top 10 largest files and categories
- Orphan file detection (optional)
- Export to CSV or JSON
- Update summary after scanning

### 🃏 Model Cards

- Color-coded borders: aquamarine = installed, orange = outdated, gold = early access / favorite creator
- Color legend bar always visible above the grid ⭐
- NSFW, Early Access (💎), and type badges
- Configurable tile size
- Quick delete from the card
- Multi-select checkboxes for batch download ⭐
- Favorite (⭐) and ban (🚫) creator directly from the card ⭐

### 🔒 Safety

- Deleted models go to the OS recycle bin by default (configurable)
- Filename sanitization — removes illegal characters automatically
- Filename length capped to prevent filesystem errors

---

## 📦 Installation

1. Open Forge Neo WebUI
2. Go to **Extensions** → **Install from URL**
3. Paste: `https://github.com/eduardoabreu81/sd-civitai-browser-neo`
4. Click **Install** and reload the WebUI

> ⚠️ This extension requires **Forge Neo**. For Forge Classic or Automatic1111, use the [anxety-solo fork](https://github.com/anxety-solo/sd-civitai-browser-plus).

---

## 📁 Auto-Organization System

The organization system sorts your models into subfolders based on their base model type, using the metadata saved alongside each file.

**Before:**
```
models/Lora/
├── model1.safetensors  (SDXL)
├── model2.safetensors  (Pony)
└── model3.safetensors  (FLUX)
```

**After:**
```
models/Lora/
├── SDXL/model1.safetensors
├── Pony/model2.safetensors
└── FLUX/model3.safetensors
```

### Auto-organize new downloads
Enable **"Auto-organize downloads"** in Settings → Model Organization.

### Organize existing models
Go to **Update Models** tab → select types → click **"📁 Organize models into subfolders"**.

### Safety
- Automatic backup before any operation
- One-click undo
- Conflict detection (skips files that already exist at destination)

---

## 📊 Dashboard & Statistics

Go to the **Dashboard** tab, select the content types you want to analyze, and click **"📊 Generate Dashboard"**.

You'll see:
- Total file count and disk usage
- Breakdown by category and architecture (Checkpoints and LORAs split by Pony, SDXL, FLUX, etc.)
- Visual progress bars and pie chart
- Top 10 largest files and categories
- Optional orphan file detection

Results can be exported as CSV or JSON.

---

## 🎨 Supported Model Types

| Architecture | Notes |
|---|---|
| SD 1.x / SD 2.x | Classic Stable Diffusion |
| SDXL | Base SDXL and derivatives |
| Pony | Pony V6 and variants |
| Illustrious | Illustrious XL |
| NoobAI | NoobAI (Illustrious-based) |
| FLUX | Dev, Krea, Kontext, Klein |
| Wan | Wan 2.2 — text/image to video |
| Qwen | Qwen-Image, Qwen-Image-Edit |
| Z-Image | Z-Image, Z-Image Turbo |
| Lumina | Lumina-Image 2.0 |
| Anima | Anima |
| Chroma | Chroma1-HD |
| Cascade | Stable Cascade |
| SVD | Stable Video Diffusion |
| Hunyuan | Hunyuan |
| Other | Catch-all; fully configurable |

Custom categories can be defined in **Settings → Model Organization** using a simple JSON pattern list.

---

## 📄 Credits

- **[sd-civitai-browser](https://github.com/Vetchems/sd-civitai-browser)** by Vetchems — original project
- **[sd-civitai-browser-plus](https://github.com/BlafKing/sd-civitai-browser-plus)** by BlafKing — foundation for this fork
- **[sd-civitai-browser-plus](https://github.com/anxety-solo/sd-civitai-browser-plus)** by anxety-solo — UI redesign and quality improvements
- **[sd-webui-civbrowser](https://github.com/SignalFlagZ/sd-webui-civbrowser)** by SignalFlagZ — creator management inspiration
- **[Forge Neo](https://github.com/Haoming02/sd-webui-forge-classic/tree/neo)** by Haoming02

---

## 📜 License

MIT — see [LICENSE](LICENSE)

---

<div align="center">

Made with ❤️ for the Stable Diffusion community

**[Report Bug](https://github.com/eduardoabreu81/sd-civitai-browser-neo/issues)** • **[Request Feature](https://github.com/eduardoabreu81/sd-civitai-browser-neo/issues)** • **[Discussions](https://github.com/eduardoabreu81/sd-civitai-browser-neo/discussions)** • **[☕ Ko-fi](https://ko-fi.com/eduardoabreu81)**

</div>
