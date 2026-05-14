# My Salah Journey 🌙

> An interactive, gamified Islamic prayer (Salah) learning application for young Muslim children, built as a single self-contained HTML file with no external dependencies.

Visit [salahjourney.netlify.app](https://salahjourney.netlify.app/) to access the game
---

## Table of Contents

1. [Overview](#overview)
2. [Motivation and Educational Rationale](#motivation-and-educational-rationale)
3. [Live Demo](#live-demo)
4. [Screenshots](#screenshots)
5. [Features](#features)
6. [Pedagogical Design](#pedagogical-design)
7. [Technical Architecture](#technical-architecture)
8. [Salah Curriculum](#salah-curriculum)
9. [Game Mechanics](#game-mechanics)
10. [Reward System](#reward-system)
11. [Accessibility and Inclusion](#accessibility-and-inclusion)
12. [File Structure](#file-structure)
13. [Installation and Usage](#installation-and-usage)
14. [Browser Compatibility](#browser-compatibility)
15. [Customisation Guide](#customisation-guide)
16. [Limitations and Future Work](#limitations-and-future-work)
17. [Contributing](#contributing)
18. [Licence](#licence)
19. [Acknowledgements](#acknowledgements)

---

## Overview

**My Salah Journey** is a standalone, offline-capable, browser-based educational game designed to teach children aged approximately 5–10 the transliterated text of the Islamic daily prayer (Salah). The entire application is delivered in a single HTML file (≈ 1,600 lines) comprising HTML5, CSS3, and vanilla JavaScript — with zero external libraries, zero network requests at runtime, and no build toolchain required.

The game guides learners through all fourteen textual components of a two-rak'ah Salah in sequence, using a three-phase learn-then-quiz loop per stage, animated feedback, a mascot guide, a progressive reward system, and a printable completion certificate.

| Property | Value |
|---|---|
| **Target age** | 5–10 years |
| **Language** | English transliteration only |
| **Technology** | Vanilla HTML5 / CSS3 / ES6 JavaScript |
| **Dependencies** | None |
| **Offline capable** | Yes |
| **File size** | ~75 KB (single `.html`) |
| **Platforms** | Desktop, tablet, mobile (responsive) |

---

## Motivation and Educational Rationale

Learning Salah is one of the earliest religious obligations introduced to Muslim children. Traditional methods of instruction — rote memorisation supported by an adult — remain effective but offer limited engagement for children accustomed to interactive media. Research in educational technology consistently demonstrates that game-based learning (GBL) improves motivation, retention, and positive attitude toward subject matter when compared with passive instruction alone (Mayer, 2019; Plass, Homer & Kinzer, 2015).

This project applies four evidence-based principles:

1. **Spaced repetition of exposure** — each textual segment is seen in a Learn phase, heard/rehearsed in a Repeat phase, and actively recalled in a Quiz phase before the learner advances.
2. **Multimodal engagement** — visual (animated text highlighting), auditory (Web Audio API tones), and kinaesthetic (touch/click interaction) channels are engaged simultaneously, consistent with Paivio's Dual Coding Theory (1991).
3. **Immediate corrective feedback** — correct and incorrect responses trigger distinct audio-visual feedback within 300 ms, reducing the window for incorrect schema formation.
4. **Intrinsic motivation scaffolding** — a progressive star-and-badge system, mascot character, and confetti celebrations are drawn from Self-Determination Theory (Ryan & Deci, 2000), targeting competence and relatedness needs to sustain engagement.

The decision to use **transliteration only** (no Arabic script, no English translation) reflects the practical reality that the target age group may not yet read Arabic, while parents/teachers can provide translation context verbally — keeping the in-app cognitive load focused solely on pronunciation recall.

---

## Live Demo

Live demo will be available

---

## Screenshots

Screenshots are available on my page


## Features

### Core Learning Features
- 14 sequential Salah stages covering the complete prayer from Takbeer to Salam
- Animated word-by-word highlighting during the Learn phase
- Three distinct quiz formats (Tap the Word, Arrange in Order, Memory Match) cycling across stages to prevent habituation
- Repeat-After-Me phase with chunked text buttons for oral rehearsal

### Visual Design
- Deep-space night-sky aesthetic with a masjid silhouette motif
- Floating star-particle canvas background (60 particles, `requestAnimationFrame` driven)
- Twelve CSS `@keyframes` animations covering mascot float, bubble pop, shake, trophy bounce, star burst, and confetti
- Day / Night (light / dark) mode toggle with CSS variable theming
- Child-friendly rounded typography via the Nunito font family (Google Fonts with graceful fallback to `Comic Sans MS`)

### Mascot & Feedback System
- 🪔 "Lumee the Lantern" mascot with speech bubble, cycling through 8 emoji variants per stage
- Overlaid feedback messages (correct / wrong) with `cubic-bezier` spring animations
- Floating `+⭐` star-burst effect anchored to the tapped element's coordinates

### Audio Engine
- Fully synthesised sound effects using the **Web Audio API** (`OscillatorNode` + `GainNode`) — no audio files, no network requests
- Three distinct sound signatures: correct answer (ascending triad), wrong answer (descending sawtooth), celebration fanfare (8-note ascending scale)
- Global mute toggle; audio context initialised on first user gesture to comply with browser autoplay policies

### Reward & Progression System
- ⭐ Stars awarded per correct answer and per stage completion
- 14 unique stage badges with custom icons and names
- Animated badge pop-up overlay on unlock
- Confetti canvas animation (160 particles, configurable duration)
- Final screen: trophy animation, star rating, full badge gallery, printable certificate

### Persistence & Parental Controls
- Game state serialised to `localStorage` as JSON on every state mutation (`currentStage`, `currentPhase`, `stars`, `badges`, `completedStages`, `soundOn`, `darkMode`)
- Continue button on home screen resumes from last completed stage
- Parent Mode modal: read-only progress summary (stars, stages completed, badges) plus a guarded progress-reset action
- Print-optimised CSS for the completion certificate (`@media print` with `no-print` class suppression)

---

## Pedagogical Design

### Three-Phase Stage Loop

Each of the 14 stages follows a fixed instructional sequence:

```
┌─────────────────────────────────────────────────────────────┐
│  PHASE 0 – LEARN                                            │
│  Full text displayed; words highlighted one-by-one at a     │
│  rate proportional to text length (300–800 ms per word).    │
│  "I'm Ready" button unlocks after full pass.                │
├─────────────────────────────────────────────────────────────┤
│  PHASE 1 – REPEAT AFTER ME                                  │
│  Text split into ~5-word chunks. Child taps each chunk      │
│  button to mark it rehearsed. Immediate audio+visual        │
│  feedback per tap. Skippable after render.                  │
├─────────────────────────────────────────────────────────────┤
│  PHASE 2 – QUIZ (rotates by stage index mod 3)              │
│  Stage % 3 == 0 → Tap the Correct Word                      │
│  Stage % 3 == 1 → Arrange Words in Order                    │
│  Stage % 3 == 2 → Memory Card Match                         │
└─────────────────────────────────────────────────────────────┘
```

On successful Quiz completion, a stage-complete animation fires, a badge is awarded, confetti plays, and the learner advances. On failure, corrective feedback appears and the quiz resets without penalising stars.

### Quiz Format Details

**Tap the Correct Word**
A randomly selected content word from the stage text is blanked out. Four options are presented (1 correct + 3 distractors drawn from other stages' vocabulary). Correct selection advances after 1.8 s; incorrect selection reveals the correct answer after 2 s and allows retry.

**Arrange Words in Order**
The first 5–7 words of the stage text are shuffled into a word bank. The learner taps words to place them in a drop zone; placed words can be removed by tapping. An explicit "Check" action validates order against the target sequence.

**Memory Card Match**
The stage text is split into 4 three-word chunks; each chunk is duplicated to create 8 face-down cards arranged in a 4×2 grid. Standard concentration rules apply: flip two cards, match = permanent reveal + match count increment, no-match = face-down after 1.2 s. Stage advances when all 4 pairs are matched.

---

## Technical Architecture

### Single-File Structure

```
salah-learning-game.html
├── <style>          CSS variables, component styles, animations (~575 lines)
├── <body>           Semantic HTML: three screens + two overlay layers
│   ├── #bgCanvas            Particle background (HTMLCanvasElement)
│   ├── #confettiCanvas      Celebration confetti (HTMLCanvasElement)
│   ├── #feedbackOverlay     Correct/wrong message overlay (div)
│   ├── #parentModal         Parent mode modal (div)
│   ├── #homeScreen          Landing / resume screen
│   ├── #gameScreen          Active gameplay screen
│   └── #finalScreen         Completion + certificate screen
└── <script>         Application logic (~1,050 lines, ES6, no modules)
```

### State Management

All mutable application state is held in a single plain JavaScript object:

```javascript
let state = {
  currentStage:     Number,   // 0–13
  currentPhase:     Number,   // 0–2
  stars:            Number,
  badges:           Array,    // [{ icon, name }, ...]
  completedStages:  Array,    // [Number, ...]
  soundOn:          Boolean,
  darkMode:         Boolean,
  playerName:       String
};
```

`saveState()` serialises this object to `localStorage` on every mutation. `loadState()` deserialises on startup with a spread-merge to guarantee forward compatibility when new keys are added.

### Rendering Pipeline

There is no virtual DOM or reactive framework. The rendering model is **imperative and pull-based**:

1. An event (button click, timer, quiz answer) mutates `state`.
2. `saveState()` persists the mutation.
3. The triggering function calls the appropriate render function (`renderStage`, `renderPath`, `renderPhasePips`, etc.).
4. Render functions write to `innerHTML` or directly manipulate `classList` / `style`.

The animation loop for background particles and confetti runs independently via `requestAnimationFrame`, completely decoupled from game logic.

### Audio Synthesis

```
Web Audio API signal chain:
  OscillatorNode (frequency, waveform type)
    └─► GainNode (attack: setValueAtTime, decay: exponentialRampToValueAtTime)
          └─► AudioContext.destination
```

All sounds are constructed programmatically at call time; no audio buffers are loaded. The `AudioContext` is lazily instantiated on the first user gesture to comply with the Chrome/Safari autoplay policy.

### Canvas Systems

**Background particles** (`#bgCanvas`): 60 `★` / `·` glyphs rendered as `fillText` on a `2d` context. Each particle has an `(x, y, dx, dy, alpha, size)` descriptor. The loop calls `clearRect` + per-particle `fillText` at ~60 fps. Viewport resize re-initialises the particle array.

**Confetti** (`#confettiCanvas`): 160 coloured rectangles with per-particle `(x, y, w, h, dx, dy, rot, drot)`. Rectangles are drawn with `translate` + `rotate` context transforms. The canvas is shown/hidden around confetti bursts and auto-hides after a configurable duration.

### CSS Architecture

All theme values are expressed as CSS custom properties on `:root`, enabling the light/dark mode toggle to function by adding/removing a single `light-mode` class on `<body>`. No JavaScript reads or writes colour values directly.

Animations are entirely CSS `@keyframes`; JavaScript only adds/removes class names to trigger them. The single exception is the `showStarBurst` function, which creates a short-lived DOM element with an inline `left`/`top` position derived from `getBoundingClientRect()`.

---

## Salah Curriculum

The following table documents all 14 stages in the order they appear in the game, matching a standard two-rak'ah Fard prayer.

| # | Stage Name | Position in Prayer | Transliteration (opening words) |
|---|---|---|---|
| 1 | Takbeer | Opening | Allahu Akbar |
| 2 | Thana | Standing (Qiyam) | Subhanaka Allahumma… |
| 3 | Ta'awudh & Tasmiyah | Before Fatiha | A'udhu billahi… Bismillah… |
| 4 | Surah Al-Fatiha | Recitation | Alhamdu lillahi rabbil 'alamin… |
| 5 | Surah Al-Ikhlas | Recitation | Qul huwallahu ahad… |
| 6 | Ruku | Bowing | Subhana Rabbiyal 'Azim |
| 7 | Qawma | Rising from Ruku | Sami'allahu liman hamidah… |
| 8 | Sajdah (1st) | First Prostration | Subhana Rabbiyal A'la |
| 9 | Jilsa | Sitting between Sajdahs | Rabbighfir li |
| 10 | Sajdah (2nd) | Second Prostration | Subhana Rabbiyal A'la |
| 11 | Tashahhud | Final Sitting (Qa'dah) | Attahiyyatu lillahi… |
| 12 | Durood Ibrahim | Final Sitting | Allahumma salli 'ala Muhammad… |
| 13 | Dua after Durood | Final Sitting | Rabbana atina fid-dunya… |
| 14 | Salam | Closing | As-salamu 'alaykum wa rahmatullah |

> **Note on scope:** This game covers the textual components of one rak'ah (repeated for the second rak'ah) plus the seated sections. Physical movements (qiyam posture, ruku angle, sajdah position) are narrated in the stage subtitle text but are not interactively taught; a supplementary physical demonstration by a parent or teacher is recommended.

---

## Game Mechanics

### Phase Transition Logic

```
renderStage()
  └─ currentPhase == 0 → renderLearnPhase()
  └─ currentPhase == 1 → renderQuizPhase1()   (Repeat After Me)
  └─ currentPhase == 2 → renderQuizPhase2()
       └─ currentStage % 3 == 0 → renderTapQuiz()
       └─ currentStage % 3 == 1 → renderArrangeQuiz()
       └─ currentStage % 3 == 2 → renderMemoryQuiz()

advancePhase()
  └─ currentPhase < 2 → currentPhase++, renderStage()
  └─ currentPhase == 2 → completeStage()

completeStage()
  └─ push to completedStages[], award bonus stars, unlock badge
  └─ show completion card, fire confetti, play celebration sound

nextStage()
  └─ currentStage++, currentPhase = 0, renderStage()
  └─ if currentStage >= 14 → showFinalScreen()
```

### Scoring

| Event | Stars Awarded |
|---|---|
| Correct answer in Tap Quiz | +1 |
| All pairs matched in Memory Quiz | +2 |
| Stage completion bonus | +2 |
| Incorrect answer | 0 (no penalty) |

Maximum possible stars across all 14 stages: **70** (14 × 5).

---

## Reward System

### Badges

Each of the 14 stages awards a unique badge on first completion:

| Stage | Badge Icon | Badge Name |
|---|---|---|
| Takbeer | 🌟 | Starter Star |
| Thana | 🌺 | Thana Star |
| Ta'awudh & Tasmiyah | 🛡️ | Shield Star |
| Surah Fatiha | 📖 | Fatiha Hero |
| Surah Ikhlas | 💎 | Ikhlas Gem |
| Ruku | ⬇️ | Ruku Champ |
| Qawma | ⬆️ | Standing Star |
| Sajdah (1st) | 🌿 | Sajdah Star |
| Jilsa | 🙏 | Forgiveness Star |
| Sajdah (2nd) | ✨ | Double Sajdah |
| Tashahhud | 💫 | Tashahhud Pro |
| Durood | ❤️ | Durood Heart |
| Dua after Durood | 🌈 | Dua Rainbow |
| Salam | 👋 | Salam Champ |

Badges are stored in `state.badges` (persisted to `localStorage`) and displayed in both the Parent Mode modal and the Final Certificate screen.

### Final Certificate

The completion screen renders a styled certificate section containing:
- "Prayer Hero" title with gold gradient typography
- A 1–5 ⭐ rating derived from `(stars / maxPossibleStars) × 5`
- Total raw star count
- Full badge gallery
- Print action (invokes `window.print()` with a `@media print` stylesheet that suppresses all non-certificate UI)

---

## Accessibility and Inclusion

| Consideration | Implementation |
|---|---|
| Touch target size | All interactive elements ≥ 44 × 44 px |
| Colour contrast | Gold (`#f7c948`) on dark navy (`#1a1a4e`) achieves ≥ 4.5:1 ratio |
| Font size | Minimum 13 px; primary interaction text 15–22 px |
| Tap highlight | `-webkit-tap-highlight-color: transparent` replaced by explicit `:active` scale transforms |
| Reduced motion | No `prefers-reduced-motion` query currently; planned for v2 (see Limitations) |
| Screen reader | Semantic HTML (`<button>`, `<div role>`) used throughout; ARIA labels planned for v2 |
| Language | All text in Latin script; no right-to-left rendering required |
| Offline | Zero network requests at runtime; suitable for use without internet access |

---

## File Structure

This project is intentionally a single-file application. A recommended repository layout is:

```
my-salah-journey/
├── salah-learning-game.html   ← The entire application
├── README.md                  ← This document
├── LICENSE                    ← Your chosen licence
└── screenshots/               ← Optional: UI screenshots for README
    ├── 01-home-screen.png
    └── ...
```

---

## Installation and Usage

### Option 1 — Local (no server required)

Open https://salahjourney.netlify.app/ 

### Option 3 — Any static host

Upload `salah-learning-game.html` to any static hosting service (Netlify, Vercel, Cloudflare Pages, AWS S3 static website, etc.). No build step or server-side runtime is required.

### Option 4 — Offline on a device

Transfer the single HTML file to a tablet or phone via USB, AirDrop, or email. Open it in the device browser. Progress will be saved locally via `localStorage`.

---

## Browser Compatibility

| Browser | Minimum Version | Notes |
|---|---|---|
| Chrome / Chromium | 66+ | Full support |
| Safari (iOS & macOS) | 11.1+ | Full support; audio requires user gesture |
| Firefox | 60+ | Full support |
| Edge (Chromium) | 79+ | Full support |
| Samsung Internet | 9+ | Full support |
| Opera | 53+ | Full support |

**Required Web APIs:**
- `Canvas API` (2D context) — particle background, confetti
- `Web Audio API` (`AudioContext`, `OscillatorNode`, `GainNode`) — sound effects
- `localStorage` — progress persistence
- `CSS Custom Properties` — theming
- `CSS @keyframes` — animations
- `requestAnimationFrame` — animation loops

No polyfills are included. All required APIs have been available in evergreen browsers since 2018.

---

## Customisation Guide

### Adding or Modifying Stages

Stages are defined in the `STAGES` array near the top of the `<script>` block:

```javascript
const STAGES = [
  {
    id: 0,
    emoji: '🤲',
    name: 'Takbeer',
    sub: 'Raise your hands and say:',
    text: 'Allahu Akbar',
    badge: { icon: '🌟', name: 'Starter Star' }
  },
  // ... 13 more stages
];
```

To add a stage: append a new object to the array with a unique `id`. To modify text: edit the `text` property. Stage count, progress dots, and quiz content all derive from this array automatically.

### Changing the Colour Scheme

All colours are CSS custom properties on `:root`:

```css
:root {
  --sky1: #1a1a4e;   /* Background gradient start */
  --sky2: #2d1b69;   /* Background gradient mid   */
  --sky3: #0f3460;   /* Background gradient end   */
  --gold: #f7c948;   /* Primary accent            */
  --teal: #00d4aa;   /* Secondary accent          */
  /* ... */
}
```

Edit these values to retheme the entire application.

### Changing the Mascot

The mascot emoji cycles through the `MASCOTS` array:

```javascript
const MASCOTS = ['🪔','🌙','⭐','🌟','🕌','🌸','✨','💫'];
```

Replace any entry with any emoji or text character.

### Adjusting the Learn Phase Speed

Word highlight interval is computed as:

```javascript
const delay = Math.max(300, Math.min(800, 5000 / words.length));
```

Increase `5000` for slower highlighting; decrease for faster. The `Math.max(300, ...)` floor prevents individual words from flashing too quickly.

### Disabling a Quiz Type

In `renderQuizPhase2`, the quiz type rotates as `currentStage % 3`. To lock all stages to a single quiz type, replace the conditional with a direct call:

```javascript
function renderQuizPhase2(stage) {
  renderTapQuiz(stage);  // Always use Tap quiz
}
```

---

## Limitations and Future Work

### Known Limitations

- **No Arabic script rendering** — by design for this age group; a toggle for older learners would be a useful addition.
- **No `prefers-reduced-motion` support** — users with vestibular disorders may find the floating animations uncomfortable. A media query hook is planned.
- **No ARIA live regions** — screen reader users cannot perceive dynamic feedback messages. Planned for v2.
- **localStorage only** — progress is device- and browser-local; no cloud sync or multi-device continuity.
- **Single-rak'ah curriculum** — the game teaches one rak'ah; a full four-rak'ah prayer requires repetition of stages 3–10 which is not yet gamified.
- **Transliteration system** — a single informal transliteration scheme is used. Families following different scholarly traditions may prefer adjusted spellings; these are editable in the `STAGES` array.

### Planned Features (v2)

- [ ] `prefers-reduced-motion` media query support
- [ ] ARIA roles and live regions for screen reader accessibility
- [ ] Audio pronunciation recordings (base64-encoded or user-supplied) as an alternative to text-only rehearsal
- [ ] Surah selection mode (teach additional surahs beyond Al-Fatiha and Al-Ikhlas)
- [ ] Multi-rak'ah simulation mode
- [ ] Parent dashboard with detailed per-stage attempt history
- [ ] Internationalisation (i18n) for Urdu, Arabic, Turkish, Malay UI strings
- [ ] Progressive Web App (PWA) manifest and Service Worker for home-screen installation
- [ ] Drag-and-drop Arrange quiz (replacing tap-to-place on desktop)
- [ ] Export progress as JSON for backup/restore

---

## Contributing

Contributions are welcome. Please follow this workflow:

1. Fork the repository.
2. Create a feature branch: `git checkout -b feature/your-feature-name`
3. Make changes to `salah-learning-game.html`.
4. Test in Chrome, Firefox, and Safari (mobile and desktop).
5. Update this `README.md` if you add, remove, or change any documented behaviour.
6. Open a Pull Request with a clear description of the change and its motivation.

### Contribution Guidelines

- Keep the application as a **single HTML file**. Do not introduce build toolchains, package managers, or external CDN dependencies.
- All transliteration additions or corrections should cite a recognised transliteration standard or scholarly source.
- New quiz types should be added as additional `renderXxxQuiz(stage)` functions and integrated into the `renderQuizPhase2` dispatcher.
- Do not introduce Arabic script without first discussing in an issue, as this has significant layout and font-loading implications.

### Reporting Issues

Please open a GitHub Issue with:
- Browser name and version
- Device type (desktop / tablet / phone) and OS
- Steps to reproduce
- Expected vs. actual behaviour
- A screenshot if applicable

---

## Licence

This project is released under the **MIT Licence**. See [`LICENSE`](LICENSE) for the full text.

```
MIT License

Copyright (c) 2025 [Aboubakar Hameed Sultan]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

## Acknowledgements

- **Nunito** typeface by Vernon Adams, Cyreal, Jacques Le Bailly — licensed under the [SIL Open Font Licence 1.1](https://scripts.sil.org/OFL). Loaded via Google Fonts with Latin-charset fallback to `Comic Sans MS`.
- Prayer text transliterations follow commonly accepted Hanafi-tradition transliteration conventions as used in mainstream English-language Islamic educational materials.
- Pedagogical framework informed by:
  - Mayer, R. E. (2019). *Computer games in education*. Annual Review of Psychology, 70, 531–549.
  - Plass, J. L., Homer, B. D., & Kinzer, C. K. (2015). Foundations of game-based learning. *Educational Psychologist*, 50(4), 258–283.
  - Paivio, A. (1991). Dual coding theory: Retrospect and current status. *Canadian Journal of Psychology*, 45(3), 255–287.
  - Ryan, R. M., & Deci, E. L. (2000). Self-determination theory and the facilitation of intrinsic motivation. *American Psychologist*, 55(1), 68–78.

---

*May this project make learning Salah a little more magical for every child who uses it.* 🌙✨
