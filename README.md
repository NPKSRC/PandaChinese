# 🐼 PandaChinese — HSK Chinese Vocabulary Trainer

A friendly, **single-file** web app for learning **HSK 1–6** Chinese vocabulary
(**~6,100 words**, plus **105 class-lesson words**) with swipeable spaced-repetition
flashcards, quizzes, and two built-in learning **games**. Read every word in
**Simplified or Traditional** characters, with meanings in **English, Thai, and Japanese**.
Meet **Bao**, your panda study buddy, who *grows up* as you learn.

![level](https://img.shields.io/badge/HSK-1--6-3f8f5b) ![words](https://img.shields.io/badge/words-6100%2B-2f6b33) ![meanings](https://img.shields.io/badge/meanings-EN%20%C2%B7%20TH%20%C2%B7%20JA-3f7cc4) ![chars](https://img.shields.io/badge/characters-Simplified%20%C2%B7%20Traditional-e08b5a) ![license](https://img.shields.io/badge/license-MIT-e7a83b)

Everything lives in one file (`index.html`) — **no build step, no server, no dependencies**.

---

## ✨ What's inside

PandaChinese is organised into **five sections** along the bottom nav:

| Section | What it does |
|---------|--------------|
| **Study** | Swipeable SRS flashcards — the core learning loop. |
| **Quiz** | Multiple-choice quizzes that feed your memory progress. |
| **Browse** | Scan the whole word list for your chosen level/course. |
| **Progress** | Your dashboard — XP, streaks, mastery, and growth breakdown. |
| **Games** | Memory Match and Tone Hero, with per-level trophies. |

---

## 📖 Study — flashcards with memory tracking

- **Swipeable cards** — swipe ← / → (or drag) between cards, tap to flip.
  The deck **shuffles on every start**, and cards are sized to show all the meaning
  languages at once without clipping.
- **Always-visible rating buttons** — rate each card **Again / Hard / Good / Easy**
  *without* having to flip it first. Ratings drive a Leitner-style spaced-repetition
  schedule, so words come back for review exactly when they're due.
- **Memory status** — every word earns a status: **New → Learning → Familiar → Mastered**.
  Star any word to flag it for extra review.
- **Simplified ↔ Traditional** — toggle between 简体 and 繁體 characters anywhere
  characters appear; conversion is handled in-app with curated fixes for tricky homographs.
- **Three meaning languages** — toggle **English / ไทย / 日本語** independently on the
  card and in Browse.
- **Pronunciation** — tap 🔊 on the front *or* back of a card to hear it (uses your
  browser's built-in Chinese voice where available).

> **About the Thai & Japanese:** these meanings are **AI-generated study drafts** — great
> for recognition and recall, though a native speaker may want to refine a few. English
> meanings come from the official HSK lists.

---

## 🎮 Games

Two games turn review into practice, each with its own setup screen showing your
progress and a **trophy + confetti** celebration when you clear a whole HSK level.

- **Memory Match** — flip-and-pair cards with zoned **Sun / Moon** card backs and
  per-scope best scores. The level locks for the duration of a round so scores stay fair.
- **Tone Hero** — identify the **tone** of a word against the clock.
  - **Single mode** — single-character tone sprints (12 questions) with a combo
    multiplier, a meaning hint, and best-score tracking.
  - **Sequence mode** — multi-syllable words, broken into syllables by a built-in
    pinyin syllabifier (with erhua support), so you tap the tone for each syllable in order.

Every game tracks **which words you've cleared**, per level, so the progress bars on
each game's setup screen fill in as you go.

---

## 📊 Progress, XP & profiles

- **Bao grows with you** — your panda evolves through several growth stages as you
  master words. Growth now comes from **both studying and playing games**, and the
  Progress screen shows a **study-vs-games breakdown** so you can see where your gains
  came from.
- **XP & growth points** — XP is a lifetime running total; growth reflects your *current*
  command of the deck (it recomputes live from how many cards you've mastered).
- **Dashboard** — a memory map, mastery-by-level, an XP chart, day streak, and recent activity.
- **Profiles** — create one or more local profiles, each with its own progress.
- **Back up & sync** — export a profile to a backup code and restore it on another
  device (no account or server needed).

---

## 📚 Courses & levels

- **Courses** — switch between the built-in **HSK** course (~6,100 words) and a
  **Class lessons** course (105 words, organised by lesson with example sentences).
  Each course keeps its own memory progress.
- **Cumulative HSK levels** — pick a single level *or* a cumulative range
  (HSK 1–2, 1–3, 1–4, 1–5 …), so you can review everything learned so far in one pool.
  The level menu is **data-driven** — it lists whatever levels exist in the data.

---

## 📁 What's in this repo

| File | What it is |
|------|------------|
| `index.html` | The complete app, vocabulary built in. **This is the only file you need to publish.** |
| `README.md` | This file. |

The whole app — UI, logic, and all HSK 1–6 vocabulary — is inlined in `index.html`.
It's mobile-first, works **offline** once loaded, and has no external dependencies.

---

## 🚀 Publish it on GitHub Pages (step by step)

No coding needed. About 5 minutes.

### 1. Create a new repository
1. Sign in to [github.com](https://github.com) (a free account is fine).
2. Click **+** top-right → **New repository**.
3. Name it, e.g. `panda-chinese`. Set it **Public**. Click **Create repository**.

### 2. Upload the file
1. On the empty repo page, click **uploading an existing file** (or **Add file → Upload files**).
2. Drag in **`index.html`**.
3. Click **Commit changes**.

### 3. Turn on GitHub Pages
1. In the repo, click **Settings → Pages**.
2. Under **Build and deployment → Source**, choose **Deploy from a branch**.
3. Branch **`main`**, folder **`/ (root)`**, click **Save**.

### 4. Visit your live app
- Wait 1–2 minutes, refresh the Pages screen, and you'll see *"Your site is live at …"*:
  ```
  https://YOUR-USERNAME.github.io/panda-chinese/
  ```
- Open it, bookmark it, share it. 🎉

> **Tip:** To update, just upload the new `index.html` and commit — the live site refreshes in a minute or two.

---

## 💾 Where is my progress stored?

Profiles, XP, streaks, game progress, and memory status are saved in your browser's
**local storage on that device**. Clearing site data erases them — so use
**Back up & sync** to copy a backup code and move a profile to another browser or device.

---

## 🧩 For maintainers — editing words & adding levels

The app reads its words from one object, `HSK_DATA`, in the inlined `<script>` near the
bottom of `index.html`. Each word looks like this:

```js
{"h":"爱","p":"ài","pos":"verb","e":"to love","th":"รัก","ja":"愛する"}
```

- `h` = hanzi · `p` = pinyin · `pos` = part of speech
- `e` = English · `th` = Thai · `ja` = Japanese

The course is **data-driven**: `buildDatasets()` reads every level key present in
`HSK_DATA`, sorts them numerically, and the level menu + course name update
automatically — **no UI code changes needed**. To add a further level, just add a new
array (e.g. `"7":[ ... ]`) to `HSK_DATA`.

Lesson-style data uses `{lesson, cn, py, en, th, jp, ex_cn, ex_py, ex_th}` and shows
example sentences on the card back.

---

## 💻 Run it locally

Just **double-click `index.html`** — it opens in your browser. No installation needed.

---

## 📚 Credits & license

- Vocabulary: the official **New HSK** word lists. Thai & Japanese meanings are
  AI-generated study aids.
- Released under the **MIT License** — free to use, modify, and share.
