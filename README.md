<div align="center">
  <img src=".github/logo.png?v=2" alt="CivitAI Browser EX"/>
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
- [Credits](#-credits)

---

## 🆕 What's New

### v0.4.1-ex — Exact Search Fix

- **Exact search fixed** — exact search (quoted term) now only applies to **Model name** queries. Previously it was incorrectly applied to Tag and User name searches, causing the CivitAI API to return no results.

### v0.4.0-ex — Download Reliability & Update Mode Improvements

- **Faster batch downloads** — the queue now processes multiple items in a single internal loop, eliminating delays between downloads caused by Gradio event round-trips.
- **Smarter integrity checks** — if a downloaded file fails SHA256 verification, the extension automatically re-queries the CivitAI API to detect silent file updates by the author. If the hash matches the new API value, the file is accepted and metadata is updated instead of failing.
- **Defensive hash search** — searching by SHA256 that returns unexpected response structures no longer crashes; the code safely handles lists and missing fields.
- **Update Mode isolation** — when loading outdated models into the Browser, Refresh and page-slider triggers no longer pull the Browser tab's filters into the Update Mode view. Update Mode state is now fully isolated.
- **Outdated list sorted by recency** — the update list is now ordered by file modification time (newest first), making it easier to prioritize which models to update first.
- **SHA256 verification speed-up** — post-download hash check now uses an 8 MB read buffer instead of 1 MB.
- **Re-trigger protection** — fixed a race condition where the Queue Trigger could fire twice for the same download, causing duplicate processing.
- **Better card sync** — card status updates are now queued when the user is on another tab and applied automatically when they return to the Browser tab.
- **More stable observers** — MutationObservers now target `document.documentElement` instead of `document.body`, preventing rare crashes during early DOM initialization.

---

## 📖 Changelog

### v0.4.1-ex — Exact Search Fix
- Batch download internal loop: process entire queue in one Gradio event to eliminate inter-item gaps.
- SHA256 silent-update detection: on mismatch, re-query `/api/v1/model-versions/{id}` to accept author-updated files.
- Defensive SHA256 search: handle list responses and missing `modelId` gracefully.
- Update Mode filter isolation: `initial_model_page` respects `update_mode` and ignores Browser-tab filters when active.
- Outdated models sorted by file modification time (descending) for easier prioritization.
- Increased SHA256 verification buffer from 1 MB to 8 MB.
- Queue Trigger re-trigger fix: return `gr.update()` without value when queue is empty.
- Card update polling: pending updates are applied when user returns to the Browser tab.
- MutationObserver stability: observe `document.documentElement` instead of `document.body`.

### v0.3.0-ex — CivitAI Domain Support & Accumulated Fixes

**CivitAI Domain Support (new in this release):**
- Added centralized domain helper to replace all hardcoded `civitai.com` URLs across the extension.
- Added `civitai_sfw_only` checkbox setting (default: off → `civitai.red`) to toggle between domains.
- Fixed search-box direct-link parser to recognize both `civitai.com` and `civitai.red` URLs.
- Updated all API calls, model page links, uploader profile links, `Referer` headers, and JSON sidecar `modelPageURL` fields to use the configured domain.

**Accumulated fixes since v0.2.4-ex:**
- Added exponential backoff retry for transient API errors (50x, timeouts).
- Added trigger word group preservation in local `.json` sidecar cache.
- Added checkpoint SHA256 cache sync with Forge on download and manual scan.
- Added safer delete flow: installed-version priority in Browser dropdown, multi-version quick-delete failsafe, and hybrid local-only card fallback for unmatched files.

### v0.2.4-ex — Trigger Word Consolidation
- Consolidated trigger words from `.safetensors` metadata, local `.json` `activation text`, and API `trainedWords`
- Added case-insensitive deduplication while preserving original order
- Model info now uses local consolidated trigger words first, with API fallback when local cache is unavailable

### v0.2.3-ex — Per-group Trigger Word Rows
- Each trigger word group gets its own row with individual copy and add-to-prompt buttons
- LORA tag row (`<lora:filename:1>`) shown as first entry in purple/monospace
- Clipboard copy with ✓ visual feedback (1.5s)
- "Add all to prompt" button when multiple groups exist

### v0.2.2-ex — Startup Crash Fix
- Fixed `NameError: name 'update_mode_banner' is not defined` — component was used as a Gradio callback output but never declared in the Browser tab layout

### v0.2.1-ex — Wildcard Download Improvements
- Own subfolder per wildcard download (sd-dynamic-prompts compatible)
- Flat zip extraction — no double-nesting when the zip has internal folders
- Skip preview/gallery images for Wildcards
- New settings: `wildcard_own_folder` (ON by default), `wildcard_organize_by_base` (OFF by default)

### v0.2.0-ex
- Update Selected — queue only checked cards, dynamic button label
- Smart version selection by base model filter
- Downloads survive screen lock / SSE disconnect (Win+L, RunPod)
- EARLY_ACCESS/NO_API: no more stray saves or unrelated file deletes
- Embeddings folder auto-detection (old and new layout support)
- Warn when both embeddings folders have content
- Fixed send-to-txt2img intermittent failure
- Guard against None json_data in session restore
- Fixed UnboundLocalError for model_folder in update flow
- Guard None preview_html in save_images
- Fixed _debug_log message format

### v0.1.0-ex
- Native Gradio 3.x compatibility
- Initial EX baseline for A1111 / Forge Classic

---

## 🗺️ Roadmap

### v0.1.0-ex — Gradio 3 Port *(complete)* ✅

### v0.2.0-ex — Stability & Feature Sync *(complete)* ✅

### v0.2.1-ex — Wildcard Improvements *(complete)* ✅

### v0.2.2-ex — Startup Crash Fix *(complete)* ✅

### v0.2.3-ex — Per-group Trigger Word Rows *(complete)* ✅

### v0.2.4-ex — Trigger Word Consolidation *(complete)* ✅

### v0.3.0-ex — CivitAI Domain Support *(complete)* ✅

### v0.4.0-ex — Download Reliability & Update Mode Improvements *(complete)* ✅

### v0.4.1-ex — Exact Search Fix *(current)*
- Exact search restricted to Model name only — CivitAI API does not support quoted search for Tag or User name

### v0.5.0-ex — Extended Features *(planned)*
- Saved search presets
- Favorites in creator/user search
- Additional browser quality-of-life improvements
- **Organization by Tag — Phase 1**: save CivitAI tags to `.json` sidecar; editable user-tags field in model panel for manual assignment
- **Organization by Tag — Phase 2**: in Manage tab, pick "anchor" tags → models with that tag sort into `<type>/<tag>/` subfolders (independent of base-model organization)

### v1.0.0-ex — First Stable Release *(planned)*
- All known issues resolved
- Full A1111/Forge Classic compatibility guarantee

---

## 🚀 Features

### 🔍 Browse & Search

- Browse CivitAI directly inside your WebUI — no tab switching needed
- Filter by content type, base model, sort order, time period, and NSFW
- Base model list is auto-updated from CivitAI API at startup
- Favorite or ban creators with instant card filtering
- Search settings persist across restarts

### 📥 Download

- Download any model, version, and file in one click
- Aria2 high-speed multi-connection downloads
- Download queue — multiple downloads run in sequence without blocking the UI
- Queue persistence — restore banner after browser disconnect; re-queue everything with one click
- SHA256 integrity check — every download verified; corrupted files caught and removed automatically
- Instant batch enqueue — queuing 10 models is as fast as queuing 1
- Cancel downloads individually or all at once

### 🔄 Model Updates

- Outdated card detection — orange border on cards with a newer version available
- Batch update from cards: select multiple and download all at once
- Retention policy on update: keep, trash, or replace
- Audit log: `ex_update_audit.jsonl`

### 🗂️ Auto-Organization

- Automatically sort new downloads into subfolders by architecture (SDXL/, Pony/, Illustrious/, etc.)
- Organize your entire existing collection in one click
- Validate organization — read-only per-file scan
- Fix misplaced files — moves flagged models with automatic backup
- Full backup & one-click rollback (keeps last 5 backups)
- Custom category patterns via JSON in Settings

### 📊 Dashboard

- Disk usage broken down by model type and architecture
- Pie chart, progress bars, percentage breakdown
- Top 10 largest files and top categories
- Export CSV / JSON

### 🔒 Safety & Integrity

- Deleted models go to OS Trash by default
- SHA256 post-download integrity check
- Filename length capped at 246 UTF-8 bytes (Linux safe)
- Illegal character sanitization

---

## 📦 Installation

1. Open your WebUI (A1111 or Forge Classic)
2. Go to **Extensions** → **Install from URL**
3. Paste: `https://github.com/eduardoabreu81/sd-civitai-browser-ex`
4. Click **Install** and reload the WebUI

**Requirements:** A1111 or Forge Classic or any Gradio 3.x SD WebUI · Python 3.10+

---

## 📄 Credits

- **[sd-civitai-browser](https://github.com/Vetchems/sd-civitai-browser)** by Vetchems — original project
- **[sd-civitai-browser-plus](https://github.com/BlafKing/sd-civitai-browser-plus)** by BlafKing — foundation for this fork
- **[sd-civitai-browser-plus](https://github.com/anxety-solo/sd-civitai-browser-plus)** by anxety-solo — UI redesign and quality improvements
- **[sd-webui-civbrowser](https://github.com/SignalFlagZ/sd-webui-civbrowser)** by SignalFlagZ — creator management inspiration
- **[Forge Classic](https://github.com/Haoming02/sd-webui-forge-classic/)** by Haoming02

---

## 📜 License

MIT — see [LICENSE](LICENSE)

---

<div align="center">

Made with ❤️ for the Stable Diffusion community

**[Report Bug](https://github.com/eduardoabreu81/sd-civitai-browser-ex/issues)** • **[Request Feature](https://github.com/eduardoabreu81/sd-civitai-browser-ex/issues)** • **[Discussions](https://github.com/eduardoabreu81/sd-civitai-browser-ex/discussions)** • **[☕ Ko-fi](https://ko-fi.com/eduardoabreu81)**

</div>
