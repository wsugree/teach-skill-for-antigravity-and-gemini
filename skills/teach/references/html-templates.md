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
      --bg: #faf9f6;
      --surface: #ffffff;
      --border: #e5e2dc;
      --text: #1c1917;
      --muted: #57534e;
      --accent: #2563eb;
      --accent-soft: #eff6ff;
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
      padding: 2rem 1.25rem 4.5rem;
      background: var(--bg);
      color: var(--text);
      font-family: "Noto Sans Thai", "Sarabun", "Leelawadee UI", "Segoe UI", system-ui, -apple-system, sans-serif;
      font-size: 1.04rem;
      line-height: 1.8;
      -webkit-font-smoothing: antialiased;
    }
    main { max-width: 780px; margin: 0 auto; }
    header {
      border-bottom: 1px solid var(--border);
      padding-bottom: 1.35rem;
      margin-bottom: 2.2rem;
    }
    .meta {
      display: flex;
      flex-wrap: wrap;
      gap: 0.5rem 1rem;
      font-size: 0.875rem;
      color: var(--muted);
      margin-bottom: 0.6rem;
    }
    .badge {
      background: var(--accent-soft);
      color: var(--accent);
      padding: 0.18rem 0.7rem;
      border-radius: 999px;
      font-weight: 700;
    }
    h1 { margin: 0.25rem 0 0.6rem; font-size: 1.95rem; line-height: 1.32; letter-spacing: -0.015em; }
    h2 { margin-top: 2.25rem; font-size: 1.35rem; line-height: 1.4; letter-spacing: -0.01em; }
    table { width: 100%; border-collapse: separate; border-spacing: 0; margin: 1.25rem 0; font-size: 0.95rem; border: 1px solid var(--border); border-radius: var(--radius); overflow: hidden; }
    th, td { border-bottom: 1px solid var(--border); padding: 0.75rem 0.9rem; text-align: left; vertical-align: top; }
    tr:last-child td { border-bottom: none; }
    th { background: var(--surface); font-weight: 700; }
    tbody tr:nth-child(even) { background: rgba(127, 127, 127, 0.04); }
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
    figure.lesson-figure {
      margin: 1.25rem 0;
      padding: 0.85rem;
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
    }
    figure.lesson-figure img {
      display: block;
      max-width: 100%;
      height: auto;
      margin: 0 auto;
      border-radius: 6px;
    }
    figure.lesson-figure figcaption {
      margin-top: 0.65rem;
      font-size: 0.86rem;
      color: var(--muted);
      line-height: 1.5;
    }
    .anim-stage {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 1rem;
      margin: 1.25rem 0;
      text-align: center;
    }
    .anim-controls, .podcast-controls {
      display: flex;
      flex-wrap: wrap;
      gap: 0.5rem;
      align-items: center;
      margin-top: 0.75rem;
    }
    .podcast-box {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 1.1rem 1.25rem;
      margin: 1.25rem 0;
    }
    .podcast-line {
      padding: 0.45rem 0.65rem;
      border-radius: 6px;
      margin: 0.35rem 0;
      font-size: 0.93rem;
      transition: background 0.2s ease;
    }
    .podcast-line.speaking {
      background: var(--accent-soft);
      border-left: 3px solid var(--accent);
    }
    .lesson-nav {
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
      align-items: center;
      gap: 0.5rem;
      padding: 0.75rem 1rem;
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      margin: 1.5rem 0;
      font-size: 0.92rem;
    }
    .lesson-nav a { text-decoration: none; font-weight: 600; }
    .lesson-nav a:hover { text-decoration: underline; }
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
      <!-- Optional verified visual downloaded to ../assets/ -->
      <figure class="lesson-figure">
        <img src="../assets/example-image.jpg" alt="Descriptive alt text" />
        <figcaption>
          <p><strong>What to notice:</strong> Explain in 2–3 clear sentences what the learner should observe in this image and how it proves the lesson's core concept.</p>
          <p class="credit"><strong>Title / Subject</strong> — Creator (Year). License: Public Domain / CC BY-SA. Source: <a href="https://commons.wikimedia.org/" target="_blank" rel="noopener noreferrer">Wikimedia Commons / Museum</a></p>
        </figcaption>
      </figure>
      <pre><code>// Minimal, realistic example</code></pre>
    </section>

    <section>
      <h2>1.5 Interactive Animation &amp; Podcast Overview</h2>
      <div class="anim-stage">
        <!-- Lightweight inline SVG + CSS/JS step-through animation -->
        <svg viewBox="0 0 600 180" width="100%" aria-label="Concept Animation">
          <rect x="20" y="30" width="560" height="120" rx="12" fill="none" stroke="currentColor" stroke-opacity="0.25" />
          <circle id="anim-dot" cx="80" cy="90" r="14" fill="var(--accent)" />
        </svg>
        <div class="anim-controls">
          <button class="reveal-btn" type="button" id="anim-step-btn">Next Step</button>
          <span id="anim-caption" class="muted">Step 1: Describe what happens here.</span>
        </div>
      </div>

      <div class="podcast-box" data-podcast-lang="en-US">
        <p><strong>Audio Podcast Overview (Auto-Generated):</strong> Listen to a 2-host conversational summary of this lesson.</p>
        <div class="podcast-controls">
          <button class="reveal-btn" type="button" id="pod-play">Play Podcast</button>
          <button class="reveal-btn" type="button" id="pod-stop">Stop</button>
          <label>Voice:
            <select id="pod-voice"></select>
          </label>
          <label>Speed:
            <select id="pod-rate">
              <option value="0.9">0.9x</option>
              <option value="1" selected>1.0x</option>
              <option value="1.2">1.2x</option>
            </select>
          </label>
        </div>
        <p id="pod-status" class="muted" style="font-size:0.85rem;margin:0.5rem 0 0;"></p>
        <div class="podcast-transcript">
          <div class="podcast-line" data-speaker="Host A" data-speak="Welcome back! Today we unpack the core mental model in under 2 minutes."><strong>Host A:</strong> Welcome back! Today we unpack the core mental model in under 2 minutes.</div>
          <div class="podcast-line" data-speaker="Host B" data-speak="And the biggest insight to remember is the key rule above."><strong>Host B:</strong> And the biggest insight to remember is the key rule above.</div>
        </div>
      </div>
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

    <nav class="lesson-nav" aria-label="Lesson navigation">
      <a href="01-previous.html">← Previous Lesson</a>
      <span>
        <a href="../reference/cheat-sheet.html">Quick Reference</a> •
        <a href="../ebook/topic-ebook.html">E-Book</a>
      </span>
      <a href="03-next.html">Next Lesson →</a>
    </nav>
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

    // Built-in offline Podcast Player (Web Speech API — voice-bound, chunked, GC-safe)
    const podBox = document.querySelector('.podcast-box');
    if (podBox && 'speechSynthesis' in window) {
      const lines = Array.from(podBox.querySelectorAll('.podcast-line'));
      const playBtn = document.getElementById('pod-play');
      const stopBtn = document.getElementById('pod-stop');
      const rateSel = document.getElementById('pod-rate');
      const voiceSel = document.getElementById('pod-voice');
      const statusEl = document.getElementById('pod-status');
      const lang = podBox.dataset.podcastLang || 'en-US';
      const langPrefix = lang.split('-')[0].toLowerCase();
      let matchingVoices = [];
      let queue = [];
      let qIdx = 0;
      let isPlaying = false;
      let activeUtter = null; // Prevent Chromium GC from cutting off speech mid-sentence

      const splitIntoChunks = (rawText, maxLen = 85) => {
        const clean = rawText.replace(/\s+/g, ' ').trim();
        if (!clean) return [];
        const parts = clean.split(/(?<=[.!?।])\s+|\s{1,}/).filter(Boolean);
        const chunks = [];
        let buf = '';
        for (const p of parts) {
          if (buf && (buf.length + 1 + p.length) > maxLen) {
            chunks.push(buf.trim());
            buf = p;
          } else {
            buf = buf ? `${buf} ${p}` : p;
          }
        }
        if (buf.trim()) chunks.push(buf.trim());
        return chunks.length ? chunks : [clean];
      };

      const loadVoices = () => {
        const all = window.speechSynthesis.getVoices();
        matchingVoices = all.filter((v) => v.lang && v.lang.toLowerCase().startsWith(langPrefix));
        if (voiceSel) {
          const prev = voiceSel.value;
          voiceSel.innerHTML = '';
          if (matchingVoices.length > 0) {
            matchingVoices.forEach((v, i) => {
              const opt = document.createElement('option');
              opt.value = String(i);
              opt.textContent = `${v.name} (${v.lang})`;
              voiceSel.appendChild(opt);
            });
            if (prev && Number(prev) < matchingVoices.length) voiceSel.value = prev;
            if (statusEl) statusEl.textContent = '';
          } else {
            const opt = document.createElement('option');
            opt.value = '';
            opt.textContent = `No ${lang} voice found`;
            voiceSel.appendChild(opt);
            if (statusEl) {
              statusEl.textContent = `Tip: No ${lang} voice is installed in this browser. Open in Microsoft Edge/Chrome for built-in natural voices, or install the ${lang} voice pack in OS Speech settings.`;
            }
          }
        }
      };
      loadVoices();
      window.speechSynthesis.onvoiceschanged = loadVoices;

      const clearHighlight = () => lines.forEach((el) => el.classList.remove('speaking'));
      const speakNext = () => {
        if (!isPlaying || qIdx >= queue.length) {
          isPlaying = false;
          activeUtter = null;
          clearHighlight();
          return;
        }
        const item = queue[qIdx];
        clearHighlight();
        item.el.classList.add('speaking');
        const utter = new SpeechSynthesisUtterance(item.text);
        activeUtter = utter;
        utter.lang = lang;
        const baseIdx = parseInt(voiceSel?.value || '0', 10) || 0;
        const hostBIdx = matchingVoices.length > 1 ? (baseIdx + 1) % matchingVoices.length : baseIdx;
        utter.voice = item.speaker === 'Host B' ? matchingVoices[hostBIdx] : matchingVoices[baseIdx];
        utter.rate = parseFloat(rateSel?.value || '1');
        utter.pitch = item.speaker === 'Host B' ? 1.06 : 0.95;
        const advance = () => {
          if (!isPlaying) return;
          qIdx += 1;
          setTimeout(speakNext, 60);
        };
        utter.onend = advance;
        utter.onerror = advance;
        window.speechSynthesis.speak(utter);
      };

      playBtn?.addEventListener('click', () => {
        window.speechSynthesis.cancel();
        loadVoices();
        if (matchingVoices.length === 0) {
          if (statusEl) {
            statusEl.textContent = `Cannot play: no ${lang} voice found in this browser (using an English voice would skip non-English text). Open in Microsoft Edge or install the ${lang} voice pack in OS Settings.`;
          }
          return;
        }
        queue = [];
        lines.forEach((el) => {
          const raw = el.dataset.speak || el.textContent.replace(/^[^:]+:\s*/, '');
          splitIntoChunks(raw).forEach((chunk) => {
            queue.push({ el, speaker: el.dataset.speaker || 'Host A', text: chunk });
          });
        });
        qIdx = 0;
        isPlaying = true;
        setTimeout(speakNext, 80);
      });
      stopBtn?.addEventListener('click', () => {
        isPlaying = false;
        activeUtter = null;
        window.speechSynthesis.cancel();
        clearHighlight();
      });
    }
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
