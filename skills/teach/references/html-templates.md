# Offline HTML Templates for `/teach`

Use these self-contained templates when generating `lessons/NN-<slug>.html` and `reference/<topic>.html`.

**Rules:**
- **Zero external dependencies:** Never add CDN `<script src="http...">`, `<link rel="stylesheet" href="http...">`, or remote fonts. Everything must work offline via `file://`.
- **Match learner language:** Translate headings, buttons, and labels to the learner's language (e.g., Thai or English).
- **Keep lessons bite-sized:** Target 5–10 minutes of active study per lesson file.

---

## 1. Lesson Template (`lessons/NN-<slug>.html`)

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Lesson NN — Title</title>
  <style>
    :root {
      --bg: #ffffff;
      --surface: #f6f8fa;
      --border: #d0d7de;
      --text: #1f2328;
      --muted: #59636e;
      --accent: #0969da;
      --accent-soft: #ddf4ff;
      --ok: #1a7f37;
      --ok-soft: #dafbe1;
      --warn: #9a6700;
      --warn-soft: #fff8c5;
      --code-bg: #f0f3f6;
      --radius: 10px;
    }
    @media (prefers-color-scheme: dark) {
      :root {
        --bg: #0d1117;
        --surface: #161b22;
        --border: #30363d;
        --text: #e6edf3;
        --muted: #8b949e;
        --accent: #58a6ff;
        --accent-soft: #13233a;
        --ok: #3fb950;
        --ok-soft: #12261e;
        --warn: #d29922;
        --warn-soft: #2b2111;
        --code-bg: #1c2128;
      }
    }
    * { box-sizing: border-box; }
    body {
      margin: 0;
      padding: 2rem 1.25rem 4rem;
      background: var(--bg);
      color: var(--text);
      font-family: system-ui, -apple-system, "Segoe UI", Roboto, sans-serif;
      line-height: 1.65;
    }
    main { max-width: 780px; margin: 0 auto; }
    header {
      border-bottom: 1px solid var(--border);
      padding-bottom: 1.25rem;
      margin-bottom: 2rem;
    }
    .meta {
      display: flex;
      flex-wrap: wrap;
      gap: 0.5rem 1rem;
      font-size: 0.875rem;
      color: var(--muted);
      margin-bottom: 0.5rem;
    }
    .badge {
      background: var(--accent-soft);
      color: var(--accent);
      padding: 0.15rem 0.55rem;
      border-radius: 999px;
      font-weight: 600;
    }
    h1 { margin: 0.25rem 0 0.5rem; font-size: 1.9rem; line-height: 1.25; }
    h2 { margin-top: 2rem; font-size: 1.3rem; }
    .card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 1.15rem 1.25rem;
      margin: 1.15rem 0;
    }
    .callout {
      border-left: 4px solid var(--accent);
      background: var(--accent-soft);
      padding: 0.9rem 1.1rem;
      border-radius: 0 var(--radius) var(--radius) 0;
      margin: 1.15rem 0;
    }
    pre {
      background: var(--code-bg);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 1rem;
      overflow-x: auto;
      font-size: 0.9rem;
    }
    code {
      font-family: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace;
      font-size: 0.9em;
    }
    .quiz-item { margin-bottom: 1.25rem; }
    .quiz-options { display: grid; gap: 0.5rem; margin-top: 0.6rem; }
    button.choice, button.reveal-btn {
      text-align: left;
      background: var(--bg);
      color: var(--text);
      border: 1px solid var(--border);
      border-radius: 8px;
      padding: 0.65rem 0.9rem;
      cursor: pointer;
      font-size: 0.95rem;
    }
    button.choice:hover, button.reveal-btn:hover { border-color: var(--accent); }
    button.choice.correct { background: var(--ok-soft); border-color: var(--ok); }
    button.choice.wrong { background: var(--warn-soft); border-color: var(--warn); }
    .feedback { margin-top: 0.5rem; font-size: 0.92rem; display: none; }
    .feedback.show { display: block; }
    .sources { font-size: 0.9rem; color: var(--muted); }
    a { color: var(--accent); }
  </style>
</head>
<body>
  <main>
    <header>
      <div class="meta">
        <span class="badge">Lesson NN</span>
        <span>~8 min</span>
        <span>Goal: <!-- tie to MISSION.md --></span>
      </div>
      <h1>Lesson Title</h1>
      <p>One-sentence mental model of what the learner will be able to do after this lesson.</p>
    </header>

    <section>
      <h2>1. Core Mental Model</h2>
      <p>Explain the concept concisely with a concrete analogy or visual diagram.</p>
      <div class="callout">
        <strong>Key Rule:</strong> State the one invariant worth memorizing.
      </div>
      <pre><code>// Minimal, realistic example</code></pre>
    </section>

    <section>
      <h2>2. Active Check</h2>
      <div class="card quiz-item" data-answer="b">
        <p><strong>Q1:</strong> Concrete scenario question that tests understanding (not trivia).</p>
        <div class="quiz-options">
          <button class="choice" data-choice="a" type="button">A) Plausible distractor</button>
          <button class="choice" data-choice="b" type="button">B) Correct reasoning</button>
          <button class="choice" data-choice="c" type="button">C) Common misconception</button>
        </div>
        <div class="feedback" data-feedback></div>
      </div>

      <div class="card">
        <p><strong>Think &amp; Reveal:</strong> Ask the learner to predict an output or explain in their own words before clicking.</p>
        <button class="reveal-btn" type="button" data-reveal-target="ans-1">Show explanation</button>
        <div id="ans-1" class="feedback">
          <p>Concise explanation tied back to the core mental model.</p>
        </div>
      </div>
    </section>

    <section>
      <h2>3. Hands-On Drill</h2>
      <div class="card">
        <p>Give 1–2 short tasks to try right now, then bring the answer or question back to chat.</p>
      </div>
    </section>

    <footer class="sources">
      <h2>Sources (from RESOURCES.md)</h2>
      <ul>
        <li><a href="https://example.com" target="_blank" rel="noopener noreferrer">Official Documentation Title</a> — Section used</li>
      </ul>
    </footer>
  </main>

  <script>
    document.querySelectorAll('.quiz-item').forEach((item) => {
      const correct = item.dataset.answer;
      const feedback = item.querySelector('[data-feedback]');
      item.querySelectorAll('button.choice').forEach((btn) => {
        btn.addEventListener('click', () => {
          item.querySelectorAll('button.choice').forEach((b) => b.classList.remove('correct', 'wrong'));
          const isRight = btn.dataset.choice === correct;
          btn.classList.add(isRight ? 'correct' : 'wrong');
          feedback.textContent = isRight
            ? 'Correct! Move on to the drill or ask in chat if anything still feels fuzzy.'
            : 'Not quite — re-check the Key Rule above and try another option.';
          feedback.classList.add('show');
        });
      });
    });

    document.querySelectorAll('[data-reveal-target]').forEach((btn) => {
      btn.addEventListener('click', () => {
        const target = document.getElementById(btn.dataset.revealTarget);
        if (target) target.classList.toggle('show');
      });
    });
  </script>
</body>
</html>
```

---

## 2. Reference Cheat-Sheet Template (`reference/<topic>.html`)

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Reference — Topic</title>
  <style>
    :root {
      --bg: #ffffff;
      --surface: #f6f8fa;
      --border: #d0d7de;
      --text: #1f2328;
      --muted: #59636e;
      --accent: #0969da;
      --code-bg: #f0f3f6;
    }
    @media (prefers-color-scheme: dark) {
      :root {
        --bg: #0d1117;
        --surface: #161b22;
        --border: #30363d;
        --text: #e6edf3;
        --muted: #8b949e;
        --accent: #58a6ff;
        --code-bg: #1c2128;
      }
    }
    * { box-sizing: border-box; }
    body {
      margin: 0;
      padding: 2rem 1.25rem 4rem;
      background: var(--bg);
      color: var(--text);
      font-family: system-ui, -apple-system, "Segoe UI", Roboto, sans-serif;
      line-height: 1.55;
    }
    main { max-width: 900px; margin: 0 auto; }
    table { width: 100%; border-collapse: collapse; margin: 1rem 0 2rem; }
    th, td {
      border: 1px solid var(--border);
      padding: 0.65rem 0.8rem;
      text-align: left;
      vertical-align: top;
    }
    th { background: var(--surface); }
    code {
      background: var(--code-bg);
      padding: 0.12rem 0.35rem;
      border-radius: 5px;
      font-family: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace;
      font-size: 0.88em;
    }
    .grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
      gap: 1rem;
    }
    .card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 10px;
      padding: 1rem;
    }
    .muted { color: var(--muted); font-size: 0.9rem; }
  </style>
</head>
<body>
  <main>
    <h1>Quick Reference: Topic</h1>
    <p class="muted">Compressed cheat sheet updated as you progress through lessons.</p>

    <h2>Syntax &amp; Patterns</h2>
    <table>
      <thead>
        <tr><th>Pattern</th><th>When to use</th><th>Example</th></tr>
      </thead>
      <tbody>
        <tr>
          <td><strong>Pattern A</strong></td>
          <td>Short condition</td>
          <td><code>example()</code></td>
        </tr>
      </tbody>
    </table>

    <h2>Glossary</h2>
    <div class="grid">
      <div class="card">
        <strong>Term</strong>
        <p class="muted">One-sentence operational definition.</p>
      </div>
    </div>
  </main>
</body>
</html>
```
