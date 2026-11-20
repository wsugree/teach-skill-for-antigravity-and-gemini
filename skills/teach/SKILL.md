---
name: teach
description: Use when the user invokes /teach or asks to learn a new programming language, framework, codebase, or complex topic over multiple sessions with structured lessons, vetted resources, and progress tracking.
---

# Stateful Personal Tutor (`/teach`)

## Overview

Turn a dedicated local directory into a persistent, multi-session interactive classroom. Rather than giving stateless chat lectures from parametric memory, `/teach` grounds every lesson in vetted external sources, builds self-contained offline HTML lessons and cheat sheets, verifies active recall before advancing, and records mastery across sessions.

**Core principle:** Learning is a stateful project. Files hold the curriculum, sources, and mastery; chat is for interviewing, active recall, and coaching.

## Workspace File Layout

```text
<learning-folder>/
├── MISSION.md                # Learning goal, current level, constraints, success criteria
├── RESOURCES.md              # Vetted official docs, guides & primary sources (cited in lessons)
├── NOTES.md                  # Tutor scratchpad: learner pace, style, recurring sticking points
├── assets/                   # Local verified public-domain/CC images for offline lessons (optional)
├── lessons/
│   └── NN-<slug>.html        # Self-contained offline interactive HTML lessons (5-10 min each)
├── podcast/
│   └── NN-<slug>.md          # Optional standalone conversational podcast script / local audio
├── reference/
│   └── <topic>.html          # Compressed cheat sheets, syntax tables & glossaries
├── ebook/
│   └── <topic>-ebook.html    # Optional printable/PDF summary e-book combining chapters
└── learning-records/
    └── NNN-<topic>.md        # ADR-style mastery logs & next Zone of Proximal Development (ZPD)
```

**Language rule:** Match the learner's language (e.g., Thai, English) across chat, Markdown state files, generated HTML lessons, and podcasts. **For research (`search_web`), always search in English (canonical global docs/museums) in addition to the learner's language** so resource quality and breadth are never limited by language.

---

## Mandatory Session Workflow

Follow these steps in order every time `/teach` is invoked. Do not skip steps under user time pressure.

### Step 0: Workspace Safety Guard
Inspect the current workspace directory first.
- **If `MISSION.md` does NOT exist** AND the directory looks like an active software repository (contains `package.json`, `Cargo.toml`, `pyproject.toml`, `go.mod`, `src/`, etc.):
  - **STOP before writing any classroom files.**
  - Warn the learner that `/teach` creates `MISSION.md`, `RESOURCES.md`, `lessons/`, `reference/`, and `learning-records/`, and recommend opening a dedicated empty folder (e.g., `~/learnings/<topic>`) instead.
  - Ask whether they want to (a) switch to a dedicated folder, or (b) proceed inside this directory anyway.

### Step 1: State Discovery
Check what already exists in the workspace and read:
1. `MISSION.md`, `RESOURCES.md`, `NOTES.md` (if present).
2. The latest file in `learning-records/` and the list of files in `lessons/` and `reference/`.

### Step 2: Onboarding Interview (When `MISSION.md` is missing)
Before teaching anything, interview the learner (use `ask_question` or 3–4 focused questions) to capture:
1. **Why / Outcome:** What real project or skill they want to achieve.
2. **Current Level:** Relevant background they already know.
3. **Constraints & Pace:** Available time per session, preferred language, and learning style.

Write the agreed goal to `MISSION.md` and initial preferences to `NOTES.md`.

### Step 3: Resource Vetting (When `RESOURCES.md` is missing or starting a new subtopic)
Never teach purely from parametric memory.
1. Use `search_web` to locate official documentation, canonical specifications, or high-trust community guides for the topic.
2. Log vetted sources in `RESOURCES.md` with title, URL, scope, and why it is trusted.
3. Every lesson generated in Step 4 **must cite** specific entries from `RESOURCES.md`.

### Step 4: Active-Learning Lesson Loop

#### 4a. Retention Check (Mandatory on Resume)
If `learning-records/` already has at least one record:
- **Do NOT jump straight into the next lesson.**
- Ask 1–2 short active-recall or prediction questions testing the previous record's core mental model.
- Address any gaps before introducing new material.

#### 4b. Generate Interactive HTML Lesson
1. Read `references/html-templates.md` in this skill directory and follow its offline-first template (zero external CDN scripts/stylesheets/fonts).
2. Create `lessons/NN-<slug>.html` targeting 5–10 minutes of focused study:
   - One clear mental model + key invariant.
   - Minimal, realistic code or concrete example.
   - **Verified Illustrative Visuals (when visuals aid understanding):** Search for real, verifiable public-domain or Creative Commons images (e.g., Wikimedia Commons, museum collections, official docs), download them locally into `assets/` so the lesson stays 100% offline-safe (no broken hotlinks), and embed them using `<figure>` with a mandatory **2-part `<figcaption>`**:
     1. **Explanatory Teaching Caption (2–3 sentences):** Point out specifically *what to notice inside the image* and *how it illustrates the concept being taught* — never leave a bare title-only label.
     2. **Attribution Line:** Title, creator/year, license, and clickable source URL (also logged in `RESOURCES.md`). Never fabricate or guess image URLs.
   - **Interactive Concept Animation (SVG/CSS/JS):** When a mechanism, flow, or timeline benefits from motion, embed a lightweight, zero-dependency inline SVG/JS interactive animation (with Play/Pause or Step controls) right inside the lesson HTML.
   - Interactive multiple-choice check + think-and-reveal prompt (inline vanilla JS).
   - **Cross-Lesson Navigation Bar:** Include clean relative links (`← Previous Lesson`, `Quick Reference`, `E-Book`, `Next Lesson →`) so the learner can move seamlessly between chapters in the browser without reopening files; when creating a new lesson, also update the previous lesson's `Next Lesson →` link.
   - 1–2 hands-on drills to bring back to chat.
   - Explicit citations from `RESOURCES.md`.
3. In chat, give a clickable `file://` link to the new lesson and a brief 3–5 line orientation. **Do not duplicate the full lesson as a wall of text in chat.**

#### 4c. Maintain Quick Reference & Multimedia E-Book (with Auto-Generated Podcast)
- When a lesson introduces reusable syntax, CLI commands, APIs, or terminology, create or update `reference/<topic>.html` using the Reference Template in `references/html-templates.md`.
- Maintain a print-to-PDF ready `ebook/<topic>-ebook.html` compiling chapter summaries and verified visuals, and **embed the Auto-Generated Podcast / Audio Overview Player inside the E-Book** so learners who prefer listening over reading can play a 2-host conversational summary of the whole course (using Web Speech API `speechSynthesis` with explicit `utter.voice` matching the learner's language, voice selector, speed control, `data-speak` natural pronunciation for non-English terms, live-highlighted transcript, and `@media print { display: none; }` so PDF exports stay clean).

#### 4d. Coach & Record Mastery
After the learner completes the drill or discusses the lesson in chat:
1. Create `learning-records/NNN-<topic>.md` recording:
   - **Mastered:** What the learner demonstrated accurately.
   - **Misconceptions Cleared:** What tripped them up and how it resolved.
   - **Next ZPD:** The exact next concept/lesson to build on this.
2. Update `NOTES.md` with any newly observed pacing or style preferences.

---

## Rationalizations & Red Flags

| Excuse / Pressure | Reality |
| :--- | :--- |
| "The user asked for a quick crash course in chat — skip files and web search." | A stateless chat dump violates `/teach`. Still run Step 0 (repo guard), vet at least 1–2 official sources via `search_web`, and keep the stateful workflow — or explicitly ask if they want to exit `/teach` mode instead of silently abandoning it. |
| "I already know Rust/Python/etc. well; no need to call `search_web` for `RESOURCES.md`." | Parametric memory drifts and cannot be cited. Always vet and log primary sources in `RESOURCES.md`. |
| "They said 'jump straight to lesson 2', so skip the retention check." | Active recall before advancing is the core learning mechanism. Take 60 seconds for 1–2 recall questions before unlocking Lesson 2. |
| "I'll just put the lesson in Markdown in chat instead of an HTML file." | Interactive offline HTML lessons (`lessons/NN-*.html`) with self-checks are a required deliverable of `/teach`. |
| "I'll update `learning-records/` later." | Sessions end unexpectedly. Write the learning record as soon as mastery of the current lesson is verified. |

### Red Flags — STOP if you catch yourself:
- Writing `MISSION.md` or `lessons/` inside a folder with `package.json` / `src/` without warning the user first.
- Writing a multi-paragraph textbook lecture directly in chat instead of creating `lessons/NN-<slug>.html`.
- Generating a lesson without citing URLs from `RESOURCES.md`.
- Advancing to `lessons/02-*.html` without checking recall of `learning-records/001-*.md`.
- Adding external CDN links (`https://cdn...`, Google Fonts) inside generated HTML files.
- Embedding guessed/unverified image URLs, images without creator/license/source attribution, or bare title-only `<figcaption>` labels without an explanatory teaching caption explaining what to notice in the image.
