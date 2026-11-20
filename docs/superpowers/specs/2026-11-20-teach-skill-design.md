# Design: `teach-skill` Antigravity Plugin

**Date:** 2026-11-20  
**Target Repo:** `f:\AIskills\teach skill` (GitHub-ready source repository)  
**Local Install Path:** `C:\Users\User\.gemini\config\plugins\teach-skill\`

---

## 1. Intent & Why

Bring the stateful, multi-session personal tutor experience of Claude Code's `/teach` skill to **Google Antigravity** as a clean, original, open-source plugin that can both be used immediately across any local learning folder and shared publicly on GitHub.

Instead of giving one-off chat answers that vanish when a session ends, `/teach` turns a dedicated workspace directory into a persistent classroom that interviews the learner, vets real external sources, generates interactive HTML lessons and quick-reference sheets, and tracks mastery across sessions.

---

## 2. Goals, Non-Goals & Anti-Goals

### Goals
- **Stateful learning workspace** using transparent local files:
  - `MISSION.md` — learner's goal, current level, constraints, and success criteria (created via onboarding interview)
  - `RESOURCES.md` — vetted, high-trust external documentation, guides, and primary sources (researched via `search_web`, cited in every lesson)
  - `lessons/NN-<slug>.html` — short, self-contained, interactive HTML lessons (explanations, visualizations, active-learning drills/quizzes)
  - `reference/<topic>.html` — compact cheat sheets, syntax tables, and glossaries for fast lookup
  - `learning-records/NNN-<topic>.md` — ADR-style mastery records tracking what is mastered, misconceptions cleared, and next Zone of Proximal Development (ZPD)
  - `NOTES.md` — tutor scratchpad for learner preferences, pace, and style
- **Antigravity-native workflow**: uses `search_web`, `ask_question`, `read_file`, `write_file`, and `edit_file` naturally without Claude Code-specific assumptions.
- **Language-adaptive**: teaches and generates materials in the learner's language (e.g., Thai or English).
- **Publishable on GitHub**: clean repository with `plugin.json`, `README.md` (with installation & usage instructions), and `LICENSE` (MIT), crediting the inspiration from stateful `/teach` workflows.
- **Installed & verified locally**: copied into `~/.gemini/config/plugins/teach-skill/` and verified via Antigravity's plugin inventory.

### Non-Goals
- Building a web server, database, or external SaaS backend (everything stays local static files).
- Copying proprietary or third-party `SKILL.md` text verbatim.
- Turning an active software production repository into a classroom (the skill warns the user if run in a non-empty codebase and recommends a dedicated folder).

### Anti-Goals (What would make this fail even if it technically works)
- **Wall-of-text lectures in chat** instead of structured, bite-sized interactive HTML lessons.
- **Hallucinated teaching from memory** without populating and citing `RESOURCES.md`.
- **Passive reading** with no active recall, drills, or retention checks before advancing.
- **Forgetting progress** across sessions because `learning-records/` or `NOTES.md` wasn't updated.

---

## 3. Constraints

- **Antigravity Plugin Schema**: `plugin.json` must use only supported top-level fields (`name`, `displayName`, `description`, `version`, `suggestedPrompts`, `author`, `repository`, `homepage`, `logo`) — no unknown fields.
- **No name collision**: verified via `GetAllPlugins` that only `superpowers` and `swarm` are installed; `teach-skill` is unique.
- **Zero external runtime dependencies**: generated `lessons/*.html` and `reference/*.html` must be completely self-contained (inline CSS/JS, works offline via `file://`).
- **TDD for skills**: `skills/teach/SKILL.md` must follow `writing-skills` discipline (baseline test without skill → write skill → verify compliance with skill).

---

## 4. Approaches Considered

| Approach | Summary | Trade-offs |
| :--- | :--- | :--- |
| **1. Antigravity-Native Global Plugin (Selected)** | Author a clean, original Antigravity plugin in `f:\AIskills\teach skill` (MIT licensed) + install globally to `~/.gemini/config/plugins/teach-skill/`, using Antigravity's native tools and a bundled HTML template. | Best fit for both Antigravity UX and open-source GitHub sharing; requires authoring and testing `SKILL.md` + template from scratch. |
| **2. Direct Port of `mattpocock/skills`** | Copy upstream `SKILL.md` and patch Claude Code tool references. | Faster initial setup, but less tailored to Antigravity tools and carries upstream licensing/attribution constraints for republishing. |
| **3. Lightweight Markdown-Only Tutor** | Same state files (`MISSION.md`, `RESOURCES.md`, `learning-records/`), but no `lessons/*.html` or `reference/*.html`. | Simpler and chat-native, but loses the signature interactive HTML lesson experience of `/teach`. |

**Why Approach 1 was selected:** Delivers full feature parity with Claude Code's `/teach` workflow, takes advantage of Antigravity's native tools, and produces a clean, original MIT-licensed repository ready for GitHub.

---

## 5. Decided Architecture

### Repository & Plugin Layout (`f:\AIskills\teach skill`)
```text
teach-skill/
├── plugin.json
├── README.md
├── LICENSE
├── docs/
│   └── superpowers/
│       └── specs/
│           └── 2026-11-20-teach-skill-design.md
└── skills/
    └── teach/
        ├── SKILL.md
        └── references/
            └── html-templates.md
```

### Core `/teach` Session Lifecycle
1. **Workspace Check**: Inspect current directory. If it looks like an active software repo (and has no `MISSION.md`), warn the learner and suggest a dedicated learning folder (or proceed if they confirm).
2. **State Discovery**: Read `MISSION.md`, `RESOURCES.md`, `NOTES.md`, and the latest files in `learning-records/`, `lessons/`, and `reference/`.
3. **Onboarding (if no `MISSION.md`)**: Interview the learner using `ask_question` / short focused questions (topic, why/goal, current experience, constraints, preferred language/pace) → write `MISSION.md` and initial `NOTES.md`.
4. **Resource Vetting (if no `RESOURCES.md` or new subtopic)**: Use `search_web` to find official docs, canonical guides, and high-trust references → log them in `RESOURCES.md` with trust notes and URLs.
5. **Lesson Loop**:
   - Check retention from the previous lesson (if any) before introducing new material.
   - Generate a focused, self-contained `lessons/NN-<slug>.html` citing `RESOURCES.md`, including interactive active-learning exercises (e.g., flip-cards, self-check quizzes, code/concept drills).
   - Update or create `reference/<topic>.html` when introducing reusable syntax, APIs, or terminology.
   - Guide the learner through practice/verification in chat, then record mastery in `learning-records/NNN-<topic>.md` and preferences in `NOTES.md`.

---

## 6. Left to the Builder

- Exact wording and structure of `skills/teach/SKILL.md` (keeping it concise and high-compliance per `writing-skills`).
- Clean, modern, dark/light-readable inline CSS & minimal JS patterns in `skills/teach/references/html-templates.md`.
- Designing the `README.md` (bilingual or English with clear Antigravity installation steps) and `plugin.json` metadata (`suggestedPrompts`, `description`).
- Running baseline & verification tests for `SKILL.md`, installing to `C:\Users\User\.gemini\config\plugins\teach-skill\`, and verifying via `GetAllPlugins`.
