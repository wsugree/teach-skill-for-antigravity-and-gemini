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
      --surface: #f5f5f7;
      --border: rgba(0, 0, 0, 0.08);
      --text: #1d1d1f;
      --muted: #6e6e73;
      --accent: #0071e3;
      --accent-soft: #eef6ff;
      --ok: #1a7f37;
      --ok-soft: #dafbe1;
      --warn: #9a6700;
      --warn-soft: #fff8c5;
      --code-bg: #f0f3f6;
      --radius: 18px;
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
    .ui-icon { width: 16px; height: 16px; stroke: currentColor; stroke-width: 1.8; fill: none; stroke-linecap: round; stroke-linejoin: round; vertical-align: -3px; margin-right: 5px; display: inline-block; }
    a { color: var(--accent); }
  </style>
</head>
<body>
  <main>
    <header>
      <div class="course-eyebrow" style="font-size:0.85rem;font-weight:600;color:var(--accent);margin-bottom:0.35rem;">Course: <!-- Course Title from MISSION.md --></div>
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
          <button class="reveal-btn" type="button" id="pod-play"><svg class="ui-icon" viewBox="0 0 24 24"><polygon points="6 4 20 12 6 20 6 4"/></svg>Play Podcast</button>
          <button class="reveal-btn" type="button" id="pod-stop"><svg class="ui-icon" viewBox="0 0 24 24"><rect x="6" y="6" width="12" height="12" rx="2"/></svg>Stop</button>
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
      <a href="01-previous.html"><svg class="ui-icon" viewBox="0 0 24 24" style="margin-right:3px;"><polyline points="15 18 9 12 15 6"/></svg>Previous Lesson</a>
      <span>
        <a href="../reference/cheat-sheet.html">Quick Reference</a> •
        <a href="../ebook/topic-ebook.html">E-Book</a>
      </span>
      <a href="03-next.html">Next Lesson<svg class="ui-icon" viewBox="0 0 24 24" style="margin-right:0;margin-left:3px;"><polyline points="9 18 15 12 9 6"/></svg></a>
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
    <div class="course-eyebrow" style="font-size:0.85rem;font-weight:600;color:var(--accent);margin-bottom:0.35rem;">Course: <!-- Course Title from MISSION.md --></div>
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


---

## 3. Editorial Magazine E-Book & Audiobook Template (`ebook/<topic>-ebook.html`)

Use this **Canva Editorial Magazine + Audiobook Reader** design pattern whenever generating a course summary E-Book (`ebook/<topic>-ebook.html`):

- **Palette (Espresso / Warm Taupe / Cream):**
  - Light mode: `--bg: #ede7e1`, `--surface: #ffffff`, `--text: #3a2e27`, `--muted: #78695f`, `--accent: #5a463a`, `--taupe: #d6ccc2`, pull-quote border `#a67b60`.
  - Dark mode (`html[data-theme="dark"]`): `--bg: #171412`, `--surface: #221d1a`, `--text: #f3ede7`, `--accent: #c9a68a`.
- **Signature Editorial Elements:**
  - Deep espresso cover card with arch-top thumbnail (`border-radius: 90px 90px 14px 14px`) and course title eyebrow.
  - Magazine chapter cards with vertical right-side taupe spine (`border-right: 12px solid #d6ccc2`), circular chapter badges (`border-radius: 50%`), arch-top photo frames (`border-radius: 120px 120px 14px 14px`), and italicized pull-quote takeaway blocks.
  - **Top App Bar:** live `Aa` font-size range slider (15–22px), 1-click Light/Dark mode toggle, link to Quick Reference, and 1-click **Print / Save as PDF** (`window.print()`).
  - **3 Interactive Tabs:**
    1. **Read E-Book** (all chapter cards + verified offline images with 2-part teaching captions + summary table)
    2. **Chapters Directory** (circular chapter cards linking both to the E-Book chapter anchor and the interactive `lessons/NN-*.html`)
    3. **Audio Podcast Player** (arch-top cover art, animated CSS waveform bars, circular espresso Play button, voice & speed selectors, live-highlighted transcript using the chunked GC-safe `speechSynthesis` engine from Section 1, hidden cleanly in `@media print`).


---

## 4. Interactive 16:9 Web Presentation Deck Template (`slides/NN-<slug>-slides.html`)

Use this zero-dependency Reveal.js-style 16:9 presentation deck whenever creating teaching slides.

**Design & QA Rules:**
- **Max 3–4 short lines of text per slide** — prioritize custom inline SVG diagrams, visual cards, and photorealistic studio images (generated via `image-generator` as 1:1 unbranded studio photos saved to `assets/`, never cartoonish art) or verified offline public-domain photos.
- **Themes:**
  - **PowerDeck Geometric Blue Diamond (Default Modern Deck):** `#FFFFFF` canvas, Cobalt Blue `#14649B`, Midnight Navy `#0B1E36`, Ice Blue `#E4F0FA`, 45°-rotated rounded diamond photo frames (`.diamond-photo` with counter-rotated `<img>`), numbered diamond badges (`.d-badge`), and full-bleed cobalt Glassmorphism break slides.
  - **Clean Scientific Modern:** Light Mode `#FFFFFF` / `#F4F7FA`, Deep Navy `#0F2A4A`, Mint `#10B981`.
- **Typography:** `"IBM Plex Sans Thai", "Prompt", "Noto Sans Thai", "Leelawadee UI", system-ui, sans-serif`.
- **Keyboard & Presenter UX:** `←` / `→` / `Space` navigate slides, `N` toggles Speaker Notes, `O` toggles Overview grid, `F` toggles Fullscreen, centered bottom control pill (never overlapping slide page numbers), and `@media print` exports clean 16:9 landscape PDF pages.
- **Visual QA Gate:** Capture rendered slide PNGs with headless Chrome/Edge (`--headless=new --screenshot`) and inspect via `read_file` to ensure zero overflow, clipping, or control overlap before delivery.

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Slides — Course Title</title>
  <style>
    :root {
      --bg: #f4f7fa;
      --slide-bg: #ffffff;
      --navy: #0f2a4a;
      --mint: #10b981;
      --mint-soft: #ecfdf5;
      --text: #1e293b;
      --muted: #64748b;
      --border: #e2e8f0;
    }
    * { box-sizing: border-box; }
    html, body {
      margin: 0; height: 100%;
      background: var(--bg);
      color: var(--text);
      font-family: "IBM Plex Sans Thai", "Prompt", "Noto Sans Thai", "Leelawadee UI", system-ui, sans-serif;
      overflow: hidden;
    }
    .deck {
      width: 100vw; height: 100vh;
      display: flex; align-items: center; justify-content: center;
      padding: 24px;
    }
    .slide {
      display: none;
      width: min(96vw, 1280px);
      aspect-ratio: 16 / 9;
      background: var(--slide-bg);
      border: 1px solid var(--border);
      border-radius: 20px;
      padding: 48px 56px;
      box-shadow: 0 20px 50px rgba(15, 42, 74, 0.08);
      flex-direction: column;
      justify-content: space-between;
      position: relative;
      overflow: hidden;
    }
    .slide.active { display: flex; }
    .slide-eyebrow { font-size: 0.9rem; font-weight: 700; color: var(--mint); letter-spacing: 0.04em; text-transform: uppercase; }
    .slide h1 { margin: 8px 0 12px; font-size: clamp(1.9rem, 3.2vw, 2.8rem); color: var(--navy); line-height: 1.25; }
    .slide h2 { margin: 6px 0 16px; font-size: clamp(1.5rem, 2.4vw, 2.1rem); color: var(--navy); line-height: 1.3; }
    .slide-body { flex: 1; display: grid; grid-template-columns: 1fr 1fr; gap: 32px; align-items: center; }
    .slide-points { list-style: none; padding: 0; margin: 0; display: grid; gap: 14px; font-size: clamp(1rem, 1.35vw, 1.25rem); line-height: 1.6; }
    .slide-points li { padding-left: 18px; border-left: 4px solid var(--mint); }
    .diagram-box { background: var(--bg); border: 1px solid var(--border); border-radius: 16px; padding: 20px; text-align: center; }
    .slide-footer { display: flex; justify-content: space-between; font-size: 0.85rem; color: var(--muted); border-top: 1px solid var(--border); padding-top: 12px; }
    .controls {
      position: fixed; bottom: 16px; right: 24px; z-index: 20;
      display: flex; gap: 8px; background: rgba(255,255,255,0.92);
      border: 1px solid var(--border); border-radius: 999px; padding: 6px 12px;
      box-shadow: 0 4px 16px rgba(15,42,74,0.1);
    }
    .controls button {
      border: none; background: transparent; color: var(--navy);
      font-weight: 600; cursor: pointer; padding: 4px 10px; border-radius: 999px; font-family: inherit;
    }
    .controls button:hover { background: var(--mint-soft); color: var(--mint); }
    .progress { position: fixed; top: 0; left: 0; height: 4px; background: var(--mint); transition: width 0.25s ease; z-index: 30; }
    .notes {
      display: none; position: fixed; left: 24px; bottom: 16px; max-width: 520px; z-index: 25;
      background: var(--navy); color: #f8fafc; padding: 14px 18px; border-radius: 14px; font-size: 0.9rem; line-height: 1.55;
    }
    .notes.show { display: block; }
    @media print {
      @page { size: landscape; margin: 0; }
      html, body { overflow: visible; height: auto; background: #fff; }
      .deck { display: block; padding: 0; height: auto; }
      .slide { display: flex !important; width: 100vw; height: 100vh; border-radius: 0; box-shadow: none; page-break-after: always; }
      .controls, .progress, .notes { display: none !important; }
    }
  </style>
</head>
<body>
  <div class="progress" id="progress"></div>
  <div class="deck" id="deck">
    <section class="slide active" data-note="Speaker note for Slide 1.">
      <div>
        <div class="slide-eyebrow">Course Title • Module 01</div>
        <h1>Presentation Title</h1>
      </div>
      <div class="slide-body">
        <ul class="slide-points">
          <li>Point 1 (concise, max 3–4 lines per slide)</li>
          <li>Point 2 (pair with SVG diagram or verified visual)</li>
        </ul>
        <div class="diagram-box">
          <!-- Custom inline SVG diagram or verified ../assets/ image -->
          <svg viewBox="0 0 360 180" width="100%"><rect x="20" y="30" width="320" height="120" rx="14" fill="#ecfdf5" stroke="#10b981" stroke-width="2"/></svg>
        </div>
      </div>
      <div class="slide-footer"><span>Course Name</span><span>1 / 1</span></div>
    </section>
  </div>
  <div class="notes" id="notes"></div>
  <div class="controls">
    <button type="button" id="prev">Prev</button>
    <button type="button" id="next">Next</button>
    <button type="button" id="toggle-notes">Notes (N)</button>
    <button type="button" id="print-pdf">PDF</button>
  </div>
  <script>
    const slides = Array.from(document.querySelectorAll('.slide'));
    const progress = document.getElementById('progress');
    const notesEl = document.getElementById('notes');
    let idx = 0;
    const render = () => {
      slides.forEach((s, i) => s.classList.toggle('active', i === idx));
      progress.style.width = `${((idx + 1) / slides.length) * 100}%`;
      notesEl.textContent = slides[idx].dataset.note || 'No speaker notes for this slide.';
    };
    const go = (d) => { idx = Math.max(0, Math.min(slides.length - 1, idx + d)); render(); };
    document.getElementById('prev').onclick = () => go(-1);
    document.getElementById('next').onclick = () => go(1);
    document.getElementById('toggle-notes').onclick = () => notesEl.classList.toggle('show');
    document.getElementById('print-pdf').onclick = () => window.print();
    window.addEventListener('keydown', (e) => {
      if (e.key === 'ArrowRight' || e.key === ' ') go(1);
      if (e.key === 'ArrowLeft') go(-1);
      if (e.key.toLowerCase() === 'n') notesEl.classList.toggle('show');
      if (e.key.toLowerCase() === 'f') document.documentElement.requestFullscreen?.();
    });
    render();
  </script>
</body>
</html>
```


---

## 5. Click-to-Zoom Uncropped Image Lightbox (Include on All Pages with Images)

Add this zero-dependency Lightbox snippet to every lesson, E-Book, and slide deck containing images so clicking any cropped or diamond-framed image opens the full, uncropped original (`object-fit: contain`) with its caption and `Esc`/click-outside to close:

```html
<!-- CSS: img { cursor: zoom-in; } .img-lightbox { display:none; position:fixed; inset:0; z-index:9999; background:rgba(10,14,22,0.88); backdrop-filter:blur(8px); align-items:center; justify-content:center; flex-direction:column; padding:24px; } .img-lightbox.open { display:flex; } .img-lightbox img { max-width:92vw; max-height:82vh; object-fit:contain; border-radius:12px; cursor:zoom-out; } -->
<div class="img-lightbox" id="img-lightbox" role="dialog" aria-modal="true">
  <button type="button" id="lb-close">✕ Close (Esc)</button>
  <img id="lb-img" src="" alt="" />
  <div id="lb-cap"></div>
</div>
```
