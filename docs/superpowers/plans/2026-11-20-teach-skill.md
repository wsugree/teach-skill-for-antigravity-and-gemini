# Teach Skill Plugin Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build and locally install the open-source `teach-skill` Antigravity plugin (bringing Claude Code's stateful `/teach` personal tutor workflow to Antigravity) inside `f:\AIskills\teach skill` and `C:\Users\User\.gemini\config\plugins\teach-skill\`.

**Architecture:** A single-skill Antigravity plugin (`teach-skill`) providing `/teach`, backed by a bundled offline HTML template reference (`skills/teach/references/html-templates.md`). The plugin source lives in `f:\AIskills\teach skill` as a GitHub-ready MIT-licensed repository and is installed globally to `~/.gemini/config/plugins/teach-skill/` for discovery in any workspace.

**Tech Stack:** Antigravity Plugin JSONC (`plugin.json`), Agent Skill Markdown (`SKILL.md`), self-contained offline HTML/CSS/JS templates, PowerShell/Git for installation and verification.

**Spec:** `docs/superpowers/specs/2026-11-20-teach-skill-design.md`

## Global Constraints

- `plugin.json` must contain only supported top-level fields (`name`, `displayName`, `description`, `version`, `suggestedPrompts`, `author`, `repository`, `homepage`, `logo`) — no unknown fields.
- Plugin `name` must be `"teach-skill"` (verified collision-free against installed plugins `superpowers` and `swarm`).
- Generated `lessons/*.html` and `reference/*.html` templates must be 100% self-contained (zero external CDN `<script>` or `<link>` dependencies; works offline via `file://`).
- `skills/teach/SKILL.md` must follow `writing-skills` TDD discipline (RED baseline test without skill → GREEN write skill → verify compliance with skill).
- Skill must adapt to the learner's language (e.g., Thai or English).
- Do not push to a remote GitHub repository unless the user explicitly asks; prepare the local git repo ready to push.

## Review Focus

- **Non-empty software repo guard:** Running `/teach` inside an active software project (with `package.json`, `src/`, etc., and no `MISSION.md`) must warn the learner and recommend a dedicated folder before writing classroom files.
- **Unverified parametric teaching:** Starting a lesson without first populating `RESOURCES.md` via `search_web` must be explicitly blocked by the skill's workflow and red-flag rules.
- **Chat wall-of-text drift:** Under user pressure ("just explain it quickly in chat"), the tutor must still maintain state files (`MISSION.md`, `lessons/*.html`, `learning-records/*.md`) rather than collapsing into a stateless chat lecture.
- **Session resume continuity:** Re-entering a folder that already has `MISSION.md` and `learning-records/` must skip onboarding, check retention from the last lesson, and advance to the next Zone of Proximal Development (ZPD).
- **Offline HTML portability:** Templates in `html-templates.md` must not reference external fonts, CDN scripts, or stylesheets so lessons render cleanly offline.

---

### Task 1: Plugin Manifest, License & Git Repository Scaffold

**Files:**
- Create: `f:\AIskills\teach skill\plugin.json`
- Create: `f:\AIskills\teach skill\LICENSE`
- Create: `f:\AIskills\teach skill\README.md`

**Interfaces:**
- Produces: Valid `plugin.json` with `name: "teach-skill"`, `displayName: "Teach Skill"`, `version: "1.0.0"`, `description`, and 3 `suggestedPrompts`.

- [ ] **Step 1: Initialize git repository if not already initialized**

Run: `git -C "f:\AIskills\teach skill" status` (if not a repo, run `git -C "f:\AIskills\teach skill" init`).

- [ ] **Step 2: Create `plugin.json`**

Write `f:\AIskills\teach skill\plugin.json` with only supported fields: `name` (`"teach-skill"`), `displayName` (`"Teach Skill"`), `version` (`"1.0.0"`), `description` (clear 1–2 sentence summary of stateful interactive tutoring), and `suggestedPrompts` (3 starter prompts).

- [ ] **Step 3: Validate `plugin.json` schema**

Run PowerShell check confirming `plugin.json` parses as JSON, `name -eq "teach-skill"`, `suggestedPrompts.Count -eq 3`, and every key is in the allowed set (`name`, `displayName`, `description`, `version`, `suggestedPrompts`, `author`, `repository`, `homepage`, `logo`, `disabled`).
Expected: `PASS: plugin.json schema valid`

- [ ] **Step 4: Create `LICENSE` (MIT) and `README.md`**

Write standard MIT `LICENSE` and a clear `README.md` covering: what `teach-skill` does, workspace files (`MISSION.md`, `RESOURCES.md`, `lessons/`, `reference/`, `learning-records/`, `NOTES.md`), Antigravity installation steps (copy/clone to `~/.gemini/config/plugins/teach-skill/`), usage (`/teach`), and inspiration credit.

- [ ] **Step 5: Commit scaffold**

Run:
```bash
git -C "f:\AIskills\teach skill" add plugin.json LICENSE README.md docs/
git -C "f:\AIskills\teach skill" commit -m "chore: scaffold teach-skill plugin manifest, license, and docs"
```

---

### Task 2: Self-Contained HTML Lesson & Reference Templates

**Files:**
- Create: `f:\AIskills\teach skill\skills\teach\references\html-templates.md`

**Interfaces:**
- Produces: Two copy-ready, offline-first HTML templates (`Lesson Template` for `lessons/NN-<slug>.html` and `Reference Template` for `reference/<topic>.html`) referenced by `skills/teach/SKILL.md`.

- [ ] **Step 1: Write `skills/teach/references/html-templates.md`**

Include:
1. **Lesson Template (`lessons/NN-<slug>.html`)**: inline CSS (clean typography, `prefers-color-scheme` light/dark support, callout boxes, code blocks), lesson header (number, title, estimated time, link back to mission/sources), core explanation sections with source citations, interactive active-learning self-check quiz/revealable answer cards (vanilla inline JS), and a hands-on drill section.
2. **Reference Template (`reference/<topic>.html`)**: compact cheat-sheet layout with filterable/scannable syntax tables, quick-copy examples, and a glossary section.

- [ ] **Step 2: Verify zero external dependencies and interactive controls**

Run PowerShell check on `skills/teach/references/html-templates.md`:
- Assert no `<script src="http` or `<link rel="stylesheet" href="http` (0 external CDN dependencies).
- Assert interactive quiz/reveal JS (`addEventListener` or `onclick`) is present.
- Assert `prefers-color-scheme` is present.
Expected: `PASS: templates offline-safe and interactive`

- [ ] **Step 3: Commit templates**

Run:
```bash
git -C "f:\AIskills\teach skill" add skills/teach/references/html-templates.md
git -C "f:\AIskills\teach skill" commit -m "feat: add self-contained offline HTML lesson and reference templates"
```

---

### Task 3: Author & TDD-Verify `skills/teach/SKILL.md`

**Files:**
- Create: `f:\AIskills\teach skill\skills\teach\SKILL.md`

**Interfaces:**
- Consumes: `skills/teach/references/html-templates.md`
- Produces: `/teach` skill with YAML frontmatter (`name: teach`, `description: "Use when..."`), full stateful learning lifecycle, and discipline guardrails.

- [ ] **Step 1: RED — Run baseline pressure test WITHOUT `SKILL.md`**

Dispatch a `research` subagent with a realistic pressure prompt:
- Scenario 1 (New topic in non-empty repo + rush pressure): *"I'm inside my work repo (`package.json`, `src/`). Teach me Rust ownership quickly — just give me a fast crash course in chat, no need to search the web or create files."*
- Scenario 2 (Resume session): *"I already have `MISSION.md` and `learning-records/001-basics.md`. Let's jump straight to lesson 2."*
Document exact baseline failures/rationalizations verbatim.

- [ ] **Step 2: GREEN — Write `skills/teach/SKILL.md`**

Write concise, high-compliance `SKILL.md`:
- YAML frontmatter: `name: teach`, `description` starting with `"Use when..."` in third person (no workflow summary in description).
- Workspace Safety Guard (warn on active software repo before writing files).
- State Discovery (`MISSION.md`, `RESOURCES.md`, `NOTES.md`, `learning-records/`, `lessons/`, `reference/`).
- Phase 1: Onboarding Interview (`ask_question` → `MISSION.md` + `NOTES.md`).
- Phase 2: Resource Vetting (`search_web` → `RESOURCES.md`, mandatory citations).
- Phase 3: Active Learning Lesson Loop (retention check on resume → generate `lessons/NN-<slug>.html` using `references/html-templates.md` → update `reference/<topic>.html` when needed → interactive practice in chat → write `learning-records/NNN-<topic>.md` and update `NOTES.md`).
- Language adaptation rule (match learner's language).
- Rationalization table & Red Flags countering the exact baseline failures from Step 1.

- [ ] **Step 3: GREEN/REFACTOR — Verify compliance WITH `SKILL.md`**

Dispatch a fresh `research` subagent given `skills/teach/SKILL.md` and the same two pressure scenarios. Confirm it:
- Warns about the active software repo before creating files.
- Refuses to skip `MISSION.md` / `RESOURCES.md` / `search_web` / HTML lesson generation under rush pressure.
- On resume, checks retention from `learning-records/001-basics.md` before advancing to lesson 2.
If any loophole appears, tighten `SKILL.md` and re-verify.

- [ ] **Step 4: Commit `SKILL.md`**

Run:
```bash
git -C "f:\AIskills\teach skill" add skills/teach/SKILL.md
git -C "f:\AIskills\teach skill" commit -m "feat: add verified /teach skill workflow"
```

---

### Task 4: Install Locally into Antigravity & Verify Live Discovery

**Files:**
- Create/Sync: `C:\Users\User\.gemini\config\plugins\teach-skill\` (copy of `plugin.json`, `README.md`, `LICENSE`, `skills/`)

**Interfaces:**
- Consumes: Completed `plugin.json`, `README.md`, `LICENSE`, `skills/teach/SKILL.md`, `skills/teach/references/html-templates.md`
- Produces: Live global Antigravity plugin discoverable as `teach-skill` with skill `teach`.

- [ ] **Step 1: Copy plugin bundle to `~/.gemini/config/plugins/teach-skill/`**

Copy `plugin.json`, `README.md`, `LICENSE`, and `skills/` from `f:\AIskills\teach skill` to `C:\Users\User\.gemini\config\plugins\teach-skill\` (excluding `.git/` and `docs/`).

- [ ] **Step 2: Verify via Antigravity `GetAllPlugins` RPC**

Run the Step 0 inventory PowerShell script against `GetAllPlugins`.
Expected: JSON output includes an entry with:
- `name`: `"teach-skill"`
- `dir`: `"teach-skill"`
- `disabled`: `false`
- `isGlobal`: `true`
- `skills`: `["teach"]`

- [ ] **Step 3: Hand back with `customization://teach-skill` link and GitHub push instructions**

Present `[teach-skill](customization://teach-skill)`, summary of files created, and ready-to-run commands for creating/pushing the GitHub repo whenever the user is ready.
