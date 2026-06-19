# 🐼 Panda HSK — Chinese Vocabulary Trainer

A friendly, single-file web app for learning **HSK 1–4** Chinese vocabulary (2000 words) with
swipeable flashcards, spaced-repetition memory tracking, quizzes, and meanings in
**English, Thai, and Japanese**. Switch between built-in **courses** — the HSK 1–3 list or your own
**class-lesson vocabulary** — right inside the app. Meet **Bao**, your panda study buddy, who *grows up* as you learn.

![level](https://img.shields.io/badge/HSK-1--4-4c9a4f) ![words](https://img.shields.io/badge/words-2000-2f6b33) ![langs](https://img.shields.io/badge/meanings-EN%20%C2%B7%20TH%20%C2%B7%20JA-3f7cc4) ![license](https://img.shields.io/badge/license-MIT-e7a83b)

## ✨ Features

- **Switchable courses (in-app)** — a **Course** selector lets you choose which vocabulary database is
  active: **HSK 1–4** (2000 words) or **Class lessons** (organised by lesson, with example sentences).
  Each course keeps its own memory progress, and the structure is built to add more courses later.

- **Compact level menu** — levels are now chosen from a single **dropdown menu** (showing the current
  group, e.g. *HSK 4 · 1000 words*) instead of a long row of buttons, keeping the screen uncluttered.
  The menu is **data-driven**: it lists whatever levels exist in the data, so adding HSK 5+ needs no UI changes.

- **Swipeable flashcards** — swipe ← / → (or drag with a mouse) to move between cards, tap to flip.
  Choose to see 汉字, 拼音, or the meaning first. The deck **shuffles on every start**, and cards are sized
  to show all three meaning languages at once without clipping.
- **Memory tracking (spaced repetition)** — after each card, rate it **Again / Hard / Good / Easy**.
  Every word earns a status — **New → Learning → Familiar → Mastered** — and reappears for review when it's due.
  Star any word to flag it for extra review.
- **Three meaning languages** — toggle **English / ไทย / 日本語** on and off independently on cards and in Browse.
- **Quizzes** — pick a type (**Meaning, Hanzi, Listening, Mixed**), a pool
  (**This level, Due for review, Marked hard**), and a length (10/20/30). Results feed back into your memory tracking.
- **Bao grows with you** — your panda evolves through six stages
  (Cub → Bamboo Scout → Clever → Scholar → Master → Sage) as you master words.
- **Profiles, XP & dashboard** — create one or more local profiles, earn XP, keep a day streak, and see a
  Progress dashboard with a memory map, mastery-by-level, a 7-day XP chart, and recent activity.
- **Back up & sync** — export a profile to a backup code and restore it on another device (no account/server needed).
- **Pronunciation** — tap 🔊 to hear any word (uses your browser's built-in Chinese voice where available). The speaker appears on **both the front and back** of each card.
- **Mobile-first & offline** — fixed, no-jump layouts, large touch targets, and works offline once loaded.

Everything lives in one file (`index.html`) — no build step, no server, no dependencies.

---

## 📁 What's in this repo

| File | What it is |
|------|------------|
| `index.html` | The complete app (vocabulary built in). **This is the only file you need to publish.** |
| `vocab-data.js` | The vocabulary as clean, editable data (EN/TH/JA), now including HSK 4 — handy for adding HSK 5+ later. |
| `README.md` | This file. |
| `USER-GUIDE.md` / `USER-GUIDE.pdf` | A friendly guide for learners. |

> **About the Thai & Japanese:** these meanings are **AI-generated study drafts**. They're great for
> recognition and recall, but a native speaker may want to refine a few. English meanings come from the official lists.
>
> **HSK 4 status:** the 1000 HSK 4 words are included with **English** meanings now; their **Thai & Japanese
> drafts are pending** and will be added in a later update. The app skips empty TH/JA gracefully, so HSK 4
> cards simply show English until then.

---

## 🚀 Publish it on GitHub Pages (step by step)

No coding needed. About 5 minutes.

### 1. Create a new repository
1. Sign in to [github.com](https://github.com) (free account is fine).
2. Click the **+** top-right → **New repository**.
3. Name it, e.g. `panda-hsk`. Set it **Public**. Click **Create repository**.

### 2. Upload the files
1. On the empty repo page, click **uploading an existing file** (or **Add file → Upload files**).
2. Drag in **`index.html`** (and optionally the rest).
3. Click **Commit changes**.

### 3. Turn on GitHub Pages
1. In the repo, click **Settings → Pages**.
2. Under **Build and deployment → Source**, choose **Deploy from a branch**.
3. Branch **`main`**, folder **`/ (root)`**, click **Save**.

### 4. Visit your live app
- Wait 1–2 minutes, refresh the Pages screen. A green box shows *"Your site is live at …"*:
  ```
  https://YOUR-USERNAME.github.io/panda-hsk/
  ```
- Open it, bookmark it, share it. 🎉

> **Tip:** To update, just upload the new `index.html` and commit — the live site refreshes in a minute or two.

---

## 🧩 Adding HSK 4 (or editing words)

The app reads its words from one object, `HSK_DATA`, in the inlined `<script>` near the bottom of `index.html`.
Each word now looks like this:

```js
{"h":"爱","p":"ài","pos":"verb","e":"to love","th":"รัก","ja":"愛する"}
```
- `h` = hanzi · `p` = pinyin · `pos` = part of speech
- `e` = English · `th` = Thai · `ja` = Japanese

**HSK 4 is already included.** To add a further level (e.g. **HSK 5**), just add a `"5":[ ... ]` array to
`HSK_DATA`. The course is now **data-driven**: `buildDatasets()` reads every level key present in `HSK_DATA`,
sorts them numerically, and the level menu + course name (e.g. *HSK 1–5*) update automatically — **no UI code changes needed**.

### Adding a whole new course (your own material)
The app keeps each course in a `buildDatasets()` function near the top of the main script. There are two
built in — `hsk` (from `HSK_DATA`) and `lesson` (from `LESSON_DATA`). To add another course, drop in a new
data object and push one more entry into the `sets` array with its `key`, `name`, `groups`, and normalised
`words` (`{h,p,pos,e,th,ja,ex}`). It will appear automatically in the **Course** selector. Lesson-style data
uses `{lesson, cn, py, en, th, jp, ex_cn, ex_py, ex_th}` and shows example sentences on the card back.

---

## 💾 Where is my progress stored?

Profiles, XP, streaks, and memory status are saved in your browser's local storage **on that device**.
Clearing site data erases them — so use **Profile → Back up & sync** to copy a backup code and move a
profile to another browser or device.

---

## 💻 Run it locally

Just **double-click `index.html`** — it opens in your browser. No installation needed.

---

## 📚 Credits & license

- Vocabulary: the official **New HSK** word lists (Levels 1–4). Thai & Japanese meanings are AI-generated study aids (HSK 4 TH/JA pending).
- Released under the **MIT License** — free to use, modify, and share.
