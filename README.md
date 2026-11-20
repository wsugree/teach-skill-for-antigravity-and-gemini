<p align="center">
  <img src="assets/logo.png" alt="Teach Skill Logo" width="128" />
</p>

<h1 align="center">Teach Skill (<code>teach-skill</code>)</h1>

<p align="center">
  <strong>Turn any folder into a persistent, interactive AI classroom for Google Antigravity.</strong><br/>
  Stateful multi-session tutoring • Vetted primary sources • Offline interactive HTML lessons • Active-recall mastery tracking
</p>

---

## Why Teach Skill?

Ordinary AI chat is great for quick answers, but terrible for *actually learning* a new language, framework, or codebase:
- Conversations scroll away and reset next session.
- Explanations come from unverified parametric memory instead of primary docs.
- Passive walls of text feel productive, yet nothing sticks a week later.

**Teach Skill** brings the stateful `/teach` workflow (inspired by Claude Code's `/teach`) to **Google Antigravity**. It treats learning as a structured project stored in transparent local files — so every session picks up right where you left off, tests what you remember, and builds a personal library of interactive lessons and cheat sheets you own forever.

---

## Key Features

- **Goal-First Onboarding (`MISSION.md`):** Interviews you on *why* you're learning, your current level, constraints, and preferred language before teaching a single concept.
- **Grounded in Vetted Sources (`RESOURCES.md`):** Uses web search to curate official documentation and canonical guides first — and cites them in every lesson instead of guessing from memory.
- **Offline Interactive HTML Lessons (`lessons/*.html`):** Generates bite-sized (5–10 min), zero-dependency HTML lessons with Light/Dark mode, multiple-choice self-checks, think-and-reveal cards, and hands-on drills that open cleanly via `file://`.
- **Living Cheat Sheets (`reference/*.html`):** Automatically maintains compact syntax tables, command references, and glossaries as you unlock new concepts.
- **Active Recall & Mastery Logs (`learning-records/*.md`):** Runs a quick retention check before advancing to the next lesson and records what you've mastered, misconceptions cleared, and your next Zone of Proximal Development (ZPD).
- **Repo Safety Guard:** Warns you before creating classroom files if you accidentally run `/teach` inside an active software repository.
- **Language-Adaptive:** Teaches and generates all files in your preferred language (Thai, English, etc.).

---

## Classroom Workspace Layout

When you run `/teach` in a dedicated folder, it builds and maintains this clean structure:

```text
my-learning-folder/
├── MISSION.md                  # Your goal, background, constraints & success criteria
├── RESOURCES.md                # Curated official docs & guides (cited in every lesson)
├── NOTES.md                    # Tutor scratchpad: your pace, style & sticking points
├── lessons/
│   ├── 01-foundations.html     # Self-contained offline interactive HTML lessons
│   └── 02-next-concept.html
├── reference/
│   └── cheat-sheet.html        # Compact syntax tables, patterns & glossary
└── learning-records/
    └── 001-foundations.md      # ADR-style mastery logs & next ZPD target
```

---

## Included Offline HTML Templates

Every generated lesson and cheat sheet uses the bundled templates in [`skills/teach/references/html-templates.md`](skills/teach/references/html-templates.md) — **100% offline-ready** (zero CDN scripts, zero remote fonts) with automatic Light/Dark mode (`prefers-color-scheme`):

1. **Interactive Lesson (`lessons/NN-<slug>.html`)**
   - Lesson badge, estimated study time (~5–10 min), and tie-in to `MISSION.md`
   - Core mental model + highlighted **Key Rule** invariant + minimal realistic code example
   - **Active Check:** clickable multiple-choice quiz with instant color-coded feedback + **Think & Reveal** explanation toggle
   - **Hands-On Drill:** 1–2 practical exercises to bring back to chat
   - **Sources Footer:** direct links back to vetted entries in `RESOURCES.md`

2. **Quick Reference (`reference/<topic>.html`)**
   - Scannable **Syntax & Patterns** comparison table (`Pattern` / `When to use` / `Example`)
   - Responsive **Glossary** cards with one-sentence operational definitions

---

## Quick Setup (Google Antigravity)

Clone or copy this repository into your Antigravity global plugins directory:

### Windows (PowerShell)
```powershell
git clone https://github.com/<YOUR_USERNAME>/teach-skill.git "$env:USERPROFILE\.gemini\config\plugins\teach-skill"
```

### macOS / Linux
```bash
git clone https://github.com/<YOUR_USERNAME>/teach-skill.git ~/.gemini/config/plugins/teach-skill
```

Antigravity discovers `teach-skill` automatically on your next turn. You can enable, disable, or inspect it anytime under **Customizations → Plugins**.

---

## How to Use

1. **Create a dedicated empty folder** for your topic:
   ```bash
   mkdir learn-rust && cd learn-rust
   ```
2. **Start your mission** in Antigravity:
   ```text
   /teach I want to learn Rust ownership and borrowing so I can build fast CLI tools
   ```
3. **Continue across sessions:** Re-open the same folder days later and run `/teach`. Your tutor will check what you remember from your last learning record before unlocking the next interactive lesson.

---

## Inspiration & License

Inspired by Matt Pocock's `/teach` stateful learning workflow for Claude Code, designed and authored from scratch for Google Antigravity's plugin and tool ecosystem.

Released under the [MIT License](LICENSE).
