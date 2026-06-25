# PandaChinese — Owner's Guide (for NPK)

This is your companion for directing changes. It has two jobs:
1. **A shared vocabulary** — a name for every part of every screen, matched to the code, so when you say "the divider in the Study options row" or "the growth split bar," we both mean the same thing.
2. **A working rhythm** that keeps changes safe and fast.

Pair this with the **annotated screen maps** (the `*-map.svg` files) — those show each name drawn onto a picture of the screen with a frame and a pointer.

---

## How to get the most out of each request

- **Attach the latest `index.html`** when you start a chat (or paste the Continuation Brief). The app changes every session, so the freshest file is the source of truth.
- **One change at a time** works best — you test on-device, then ask for the next. That's already your rhythm; it keeps every step reversible.
- **Name the screen + the element.** "On **Study**, make the **Restart** button bigger" beats "make that button bigger." Use the names in the glossary below.
- **Say the goal, not just the mechanism.** "I want to compact the space" let me reflow the whole row sensibly, rather than nudging one margin.
- **Point with words you can see.** Colour ("the amber bar"), position ("top-right of the options row"), and the on-screen label ("the ⏰ Study due button") are all great anchors. The code id (in `monospace` below) is the most precise of all if you want to use it.

## The naming glossary

Each entry is **Name** — what it looks like — `codeId`.

### Global (every screen)
- **Brand bar** — panda logo + "Panda HSK / Learn Chinese with Bao" at the very top — `.brand`
- **Buddy avatar / profile button** — tap to switch or add profiles — opens `#acctModal`
- **Course bar** (a.k.a. scope bar) — the row of course/level pills under the brand — `#dsBar`
- **Bottom nav** — the five tabs — `.nav`; tabs: Study `data-view="study"`, Quiz `quiz`, Browse `browse`, Progress `progress`, Games `games`

### 📖 Study screen — `#view-study`
- **Meanings bar** — "Meanings: English / ไทย / 日本語" — `#langbarStudy`
- **Speech hint** — the line the buddy "says" — `#studySpeech`
- **Options row** — the compact row holding chips + divider + actions + buddy — `.study-optrow`
  - **Front-side chips** — 汉字 / 拼音 / Meaning first — `#frontOpts`
  - **Script chips** — 简 Simplified / 繁 Traditional — `#scriptOpts`
  - **Options divider** — the thin vertical line — `.optrow-div`
  - **Study actions** — the light-blue button pair — `.study-actions` → **Shuffle** `#shuffleBtn`, **Restart** `#resetStudyBtn`
  - **Study buddy** — the mascot on the right — `#pandaStudy`
- **Study progress bar** — fill + "1 / 300" counter — `#studyBar`, `#studyCounter`
- **Flashcard** — the flip card — `#card` (front `#cardFront`, back `#cardBack`)
- **Rating buttons** — Again / Hard / Good / Easy — `#rateRow`
- **Card nav** — the round ‹ › arrows — `#prevBtn`, `#nextBtn`
- **Study due button** — "⏰ Study due (N)" in the bottom controls — `#dueBtn`

### ❓ Quiz screen — `#view-quiz`
- **Quiz setup panel** — the start screen — `#quizSetup`
- **Quiz-type selector** — Meaning / Hanzi / Listening — `#qType`
- **Quiz speech hint** — `#quizSpeech`

### ☰ Browse screen — `#view-browse`
- **Status filter** — New / Learning / Familiar / Mastered chips — `#statusFilter`
- **Word list** — the scrollable list — `#browseList`

### 📈 Progress screen — `#view-progress`
- **Big buddy** — the large mascot — `#pandaBig`
- **Stage name / caption** — "Cub", and its tagline — `#stageName`, `#stageCap`
- **Growth meter** — the gold→jade bar to the next stage — `#growBar`; "N growth" `#growNow`, "Next: N" `#growNext`
- **Growth split** — the two-tone Studying/Games bar + legend — `#growSplit`
- **Stat tiles** — Total XP / Day streak / Words mastered / Quiz accuracy — `#statXp`, `#statStreak`, `#statMastered`, `#statAcc`
- **Memory map** — New/Learning/Familiar/Mastered coverage bars — `#memoBars`

### 🎮 Games screen — `#view-games`
- **Games hub** — the menu of game cards — `#gamesHub`
- **Memory Match card** — `#gameCardMM`
- **Tone Hero card** — `#gameCardTH` (its icon mirrors your buddy's stage — `#gameCardTHIc`)

### Profiles
- **Accounts modal** — the profile manager pop-up — `#acctModal` (list `#acctList`, + New profile `#newAcctBtn`)

---

## Two things parked for later (your notes)

1. **Growth tops out too early.** Marking *Easy* through one HSK level already maxes the buddy to **Sage**, even with five levels left. Worth rescaling the stage thresholds to the full ~6,205-word deck (or tying top stages to multi-level coverage) so the buddy keeps growing.
2. **No flashcard-completion celebration.** Games pop a trophy when a level is cleared; Study has no equivalent. Worth adding a celebration when all flashcards in a level are finished.

When you're ready to tackle either, just say "let's do parked item 1/2" and point me at the latest file.

## Quick reference: the two meters

- **XP** = lifetime points, only goes up. Shown as **Total XP**.
- **GROWTH** = live score from how many words you've mastered (plus a little from games). This is what moves your **buddy's stage**. Studying is the main driver; games can help but can't reach the top stages alone.
