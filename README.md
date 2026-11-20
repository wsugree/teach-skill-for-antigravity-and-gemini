# Teach Skill (`teach-skill`)

A stateful, multi-session personal tutor plugin for **Google Antigravity** (inspired by the `/teach` stateful learning workflow in Claude Code).

Instead of giving one-off chat answers that vanish when a session ends, `/teach` turns any dedicated folder into a persistent, interactive classroom that remembers your goals, grounds every lesson in vetted external sources, builds self-contained offline HTML lessons and cheat sheets, and tracks your mastery over time.

## How It Works

Open an empty folder dedicated to the topic you want to learn and invoke `/teach`. The skill manages your classroom through transparent local files:

```text
my-learning-folder/
├── MISSION.md                # Your learning goal, current level, constraints & success criteria
├── RESOURCES.md              # Vetted external docs, guides & primary sources (cited in lessons)
├── NOTES.md                  # Tutor scratchpad for your pace, learning style & preferences
├── lessons/
│   ├── 01-foundations.html   # Short, self-contained interactive HTML lessons (works offline)
│   └── 02-next-concept.html
├── reference/
│   └── cheat-sheet.html      # Compact syntax tables, commands & glossaries
└── learning-records/
    └── 001-foundations.md    # ADR-style mastery logs & next Zone of Proximal Development
```

### Key Principles
- **Interview First:** Before teaching anything, `/teach` interviews you to capture *why* you are learning and what success looks like in `MISSION.md`.
- **Grounded, Not Hallucinated:** Searches and vets trustworthy external sources into `RESOURCES.md` and cites them in every lesson.
- **Active Learning:** Generates interactive offline HTML lessons with self-check quizzes, revealable answers, and hands-on drills — then verifies retention before advancing.
- **Language-Adaptive:** Teaches and generates all workspace files in your preferred language (Thai, English, etc.).

## Installation (Google Antigravity)

Clone or copy this repository into your Antigravity global plugins directory:

### Windows (PowerShell)
```powershell
git clone <YOUR_REPO_URL> "$env:USERPROFILE\.gemini\config\plugins\teach-skill"
```

### macOS / Linux
```bash
git clone <YOUR_REPO_URL> ~/.gemini/config/plugins/teach-skill
```

Antigravity discovers the plugin automatically on your next turn. You can manage it anytime from the **Customizations → Plugins** page.

## Usage

1. Create and open a dedicated empty folder for your topic (e.g., `mkdir learn-rust && cd learn-rust`).
2. Start a conversation in Antigravity and type:
   ```text
   /teach I want to learn Rust ownership and borrowing so I can build CLI tools
   ```
3. Return to the same folder in future sessions and type `/teach` to review previous concepts and continue where you left off.

## Inspiration & License

Inspired by Matt Pocock's `/teach` stateful learning workflow for Claude Code, re-authored from scratch for Google Antigravity's plugin and tool ecosystem.

Released under the [MIT License](LICENSE).
