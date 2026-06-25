# PandaChinese — Continuation Brief (paste into a new chat to resume)

**Use:** Paste this whole file at the start of a new chat so Claude has full context without re-reading the app. Attach the latest `index.html` too.

---

## 1. What this project is

PandaChinese (the in-app brand bar still reads **"Panda HSK — Learn Chinese with Bao"**) is a single-file HSK Chinese-vocabulary learning web app.

- **One file:** `index.html` (~1 MB). All HTML, CSS, JS, and the entire vocabulary are inlined. No build step, no frameworks.
- **Deploy:** GitHub Pages (custom domain). The file *is* the app — edit it, commit, done.
- **Storage:** `localStorage`, with multi-profile support (each buddy profile keeps its own XP, streak, card memory, game clears).
- **Roles:** NPK is the non-technical Product Owner and makes all design/UX calls; Claude writes all the code.

## 2. Architecture facts

- **Vocabulary:** HSK 1–6 = 6,100 words, plus 105 class-lesson words = **6,205 total**. Built in `buildDatasets()`; looked up via `WORD_BY_ID`; full set is `ALL_WORDS`. Per-level counts: HSK1 = 300, HSK2 = 200, HSK3 = 500, HSK4 = 1,000, HSK5 = 1,600, HSK6 = 2,500.
- **Five tabs** (bottom nav): Study, Quiz, Browse, Progress, Games. A **course/scope bar** (`#dsBar`) at the top lets the learner pick HSK level(s) or the lessons course.
- **Two progress meters:**
  - **XP** (`a.xp`) — append-only lifetime total, only ever grows (via `awardXP`).
  - **GROWTH** (`gp`) — *recomputed live* every render from card mastery + game clears; never stored. This is what drives the mascot's **stage**.
- **Mascot stages** (`STAGES`, via `stageFromGP`): Cub (0) → Scout (20) → Quick Learner (60) → Scholar (140) → Master (300) → Sage (600).
- **`growthPoints(acct)`** returns `{gp, mastered, familiar, studyGP, gameGP}`.
  - `studyGP` = `mastered×3 + familiar` (from SRS card status).
  - `gameGP` = from `gameGrowth()`: each game's clears normalize so a *full* clear of all games = exactly 100, plus milestone bonuses capped at 50 (so games contribute at most 150 — they can lift the buddy to Scholar but **never to Master/Sage on their own**; only studying does that).
- **SRS:** Leitner-style flashcards rated **Again / Hard / Good / Easy**. Status buckets: new / learning / familiar / mastered.
- **Games:** Memory Match (`mm`, all words) and Tone Hero (`th`, single-character words only). Per-level clear tracking lives in `a.cleared`; `markCleared` + `checkGameCompletion` fire per-level and whole-course trophy/confetti modals.

## 3. What changed in the most recent sessions

1. **Game achievements feed Growth** ("Section 4"): game clears now contribute to the buddy's GROWTH, derived live from `a.cleared` (idempotent, retroactive, capped at 150).
2. **Progress screen — study-vs-games split:** under the growth meter, a slim two-tone bar (jade = studying, amber = games) plus a legend "Studying N · Games N". Hidden until games actually contribute.
3. **Study — rating buttons always visible:** Again/Hard/Good/Easy no longer wait for the card flip; the learner can rate immediately.
4. **Study — hint reworded** to "Rate how well you knew it — tap the card to peek at the answer."
5. **Study — compact options row:** the buddy moved up beside the chips. Row now reads: **option chips** (汉字/拼音/Meaning first + 简/繁) · **thin vertical divider** · **🔀 Shuffle / ↺ Restart** (light-blue buttons) · **buddy**. **⏰ Study due** stays in the bottom controls row.

## 4. Room for improvement (NOT urgent — parked notes)

> **#1 — Growth ceiling is reached far too early.**
> Rating **Easy** through a *single* HSK level (~300 words in HSK1 → `studyGP ≈ 900`) already pushes the buddy to **Sage**, even with five HSK levels still unlearned. The `STAGES` thresholds were calibrated for a much smaller deck. Consider rescaling the thresholds to the full ~6,205-word corpus, and/or tying the top stages to **coverage across multiple levels** rather than raw mastered-count, so the buddy keeps growing as more of HSK is actually learned.

> **#2 — No celebration when a learner finishes all flashcards in a level.**
> Games already fire per-level trophy + confetti modals, but **Study mode has no pop-up** when a learner completes (or masters) all flashcards in an HSK level. Add a celebration modal + confetti on full-level flashcard completion, mirroring the existing games-trophy pattern.

## 5. Other open items / tuning knobs

- **Tone Hero Sequence (`ts`)** is a future game mode. The growth code already has a `ts` stub in `GAME_KINDS` + `gameEligibleForGrowth` (multi-syllable words); the self-normalizing math auto-recalibrates once it's wired.
- **`GAME_MILESTONES`** rung values and the **100/50** per-clear-vs-milestone split are single-line tuning knobs at the top of the growth block.

## 6. House rules

- Deliver **one complete, syntax-checked `index.html`** per session.
- Validate before delivering: `node --check` on **both** `<script>` blocks; confirm `<div>` open/close balance is unchanged or intentional.
- Make **surgical, reversible** edits. Keep the **jade / coral / amber** palette and **Outfit** typeface — these are non-negotiable.
- Don't dump the inlined vocabulary in tool output; use targeted `grep`/`sed` on code regions.
- NPK tests on-device between changes and then requests the next one.
