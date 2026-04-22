# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Static HTML setup guides for FF Office Hours: three standalone pages walking users through installing AI CLIs (`index.html`), Git (`git-setup.html`), and WSL2 (`wsl2-setup.html`). No build system, no package manager, no server — each file is plain HTML with inline `<style>` and `<script>`. Open directly in a browser, or serve with any static host.

## Architecture conventions

- **Each page is self-contained.** Inline CSS and JS live inside each HTML file — there is no shared stylesheet or script. When tweaking a style token, update the `:root { --var }` block inside the specific file you're editing.
- **Cross-page links are flat and relative** (e.g. `<a href="git-setup.html">`). Files must stay at the repo root or links will break.
- **Tab widgets use a `data-group` scoping pattern.** A single page contains multiple independent OS-tab widgets (macOS / Windows / Linux pickers per install step). The `showOS(btn, os)` JS function scopes the active-state toggle to elements sharing the same `data-group` attribute within the same `.step` container. When adding a new OS-tab widget, give every `.os-tabs` and every `.os-content` inside that step a unique `data-group` value — otherwise clicks will flip tabs in unrelated steps.
- **Tool tabs (Claude / Gemini / Codex)** use `showTool(tool)` with `data-tool` attributes and `.tool-section` IDs matching the tool name (`#claude`, `#gemini`, `#codex`). Used on `index.html` (page-level) and inside a single step on `wsl2-setup.html` — the `showTool` function is page-global and toggles all `.tab-btn`/`.tool-section` elements, so don't add a second unrelated tool-picker to the same page.
- **Accent colors are semantically tied to tools:** `--accent-claude` (warm tan), `--accent-gemini` (blue), `--accent-codex` (green). The active tab's underline is driven by a `.tab-btn[data-tool="..."].active` rule — keep these in sync if you add a new tool.

## Common tasks

- **Preview changes:** open the HTML file directly in a browser (`file://`) or run any static server from the repo root, e.g. `python3 -m http.server`.
- **No lint, no tests, no CI.** Changes are validated by loading the page and clicking through the tabs.
