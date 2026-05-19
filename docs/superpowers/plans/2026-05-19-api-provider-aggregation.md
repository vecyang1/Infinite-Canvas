# API Provider Aggregation Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Make every remote image provider, including VectorEngine and ModelScope, available through one API provider picker while keeping Local as the only separate engine.

**Architecture:** Keep backend provider records as the source of truth from `/api/config`. Frontend helpers will expose image-capable API providers without excluding `modelscope`, ordered as primary first, then non-ModelScope providers such as `vt-260518`, then ModelScope. ModelScope-specific LoRA and preset UI remains on the existing ModelScope canvas node path.

**Tech Stack:** Static HTML/JS frontend, FastAPI backend provider schema, Python unittest static contract tests, local Playwright/browser QA.

### Task 1: Lock Provider Aggregation Contract

**Files:**
- Modify: `tests/test_agent_provider_config.py`
- Modify: `docs/API.md`

- [x] Add static tests that fail while `static/zimage.html` and `static/canvas.html` filter out `modelscope` from generic API image providers.
- [x] Add static tests that require provider ordering helpers to prefer primary providers, then non-ModelScope providers, then ModelScope.
- [x] Update API docs to state that ModelScope is an API provider with provider-specific extras, not a separate engine tab.

### Task 2: Update Text-to-Image Console

**Files:**
- Modify: `static/zimage.html`
- Modify: `static/i18n.js`

- [x] Replace the three-way `Local / API / ModelScope` engine switcher with `Local / API`.
- [x] Make the API provider dropdown include ModelScope and VectorEngine.
- [x] Preserve VT/VectorEngine as the default when no provider is explicitly selected by ordering non-ModelScope providers ahead of ModelScope.
- [x] Migrate old `cloud` engine state to API with ModelScope selected so existing ModelScope users are not silently switched to VT.
- [x] Remove the unreachable direct Text-to-Image ModelScope `/generate` path after ModelScope became a generic API provider option.
- [x] Keep API generation through `/api/canvas-image-tasks`, including size and concurrent task behavior.

### Task 3: Update Canvas API Generator

**Files:**
- Modify: `static/canvas.html`

- [x] Update `imageApiProviders()` so it includes ModelScope.
- [x] Add a shared provider ordering helper so Canvas API generator defaults to VT/VectorEngine ahead of ModelScope unless a provider is marked primary.
- [x] Prefer GPT Image models for Canvas API generator defaults when a provider also lists Gemini image-preview models.
- [x] Leave the ModelScope-specific `msgen` node intact for LoRA/preset workflows.

### Task 4: Verify, QA, and Publish

**Files:**
- Modify: `CHANGELOG.md`

- [x] Run the focused failing tests after writing them and confirm RED.
- [x] Implement the frontend/docs changes and rerun focused tests to confirm GREEN.
- [x] Run provider/runtime regression tests, `git diff --check`, and `gitnexus detect-changes`.
- [x] Start or reuse `mp2`, verify the rendered Text-to-Image console shows `Local / API`, and prove the API provider dropdown includes VT/VectorEngine and ModelScope.
- [x] Commit and push to `origin/main`.
