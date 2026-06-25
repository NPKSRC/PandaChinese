# PandaChinese — Handoff: Game Achievements → Mascot Growth

> Paste this whole document into the new chat, **and upload the current `index.html`** (the version that already has Tone Hero v2 Sequence mode). This file is the spec; the new session should build from it without re-deriving anything.

---

## 0. How to start the new chat
1. Upload `index.html` (current working file — has Study / Quiz / Browse / Progress / Games, where Games = Memory Match + Tone Hero **Single 字** + Tone Hero **Sequence 词**).
2. Paste this document.
3. Ask Claude to build **Section 4** below. The growth feature is **not yet built** — everything before Section 4 is context/ground-truth.

---

## 1. Project context
- **PandaChinese** (aka "Panda HSK · Learn Chinese with Bao") — a single-file `index.html` (HTML/CSS/JS, no build step), deployed on GitHub Pages, all state in `localStorage`, ~6,100 HSK 1–6 words inlined with EN/TH/JA.
- NPK is the non-technical Product Owner (makes design/UX calls); Claude writes all code.
- House style: deliver **one complete, syntax-checked file per session**; prose over bullets; iterate on results.

---

## 2. The two meters (this is the crux)
The app has **two separate progress meters**. They must not be confused.

**A) XP (`a.xp`)** — an **append-only lifetime total**. Only ever increases; awarded the moment something happens; never recomputed. Drives the "X XP" chip, the Progress stat, the profile picker, the daily XP bar chart (`xpByDay`), and the activity log. **Games already feed XP.**

**B) Growth (`gp`) — the mascot's stage** — a **live-recomputed, derived value**, NOT stored. Computed every render as:
```
growthPoints(acct) → gp = mastered×3 + familiar      // from SRS card status only
```
It rewards *retained knowledge* and **can go down** if you forget words. **Games do NOT feed growth today** — this is what we're changing.

### Mascot stage ladder (`STAGES`, thresholds in growth points)
| Stage | Growth needed | Gap |
|---|---|---|
| Cub | 0 | — |
| Scout | 20 | 20 |
| Quick Learner | 60 | 40 |
| Scholar | 140 | 80 |
| Master | 300 | 160 |
| Sage | 600 | 300 |

Full climb Cub→Sage = **600 points**; caps at Sage. Gaps widen with height, so a fixed point boost lifts a beginner more rungs than a veteran.

---

## 3. Current XP system (ground truth — already built, don't change)
Core: `awardXP(amount, msg, kind)` adds to `a.xp`, adds to `xpByDay[today]`, and (if `msg`) prepends a log entry (capped at last 40).

| Activity | Award |
|---|---|
| Study — Again / Hard / Good / Easy | +2 / +3 / +5 / +6 |
| First time a card becomes **Mastered** (study or quiz) | +20 (one-time per card) |
| Quiz — correct / wrong | +10 / +2 |
| Memory Match (per finished game) | `pairs×3` + accuracy bonus up to `pairs×2` |
| Tone Hero · Single (per game) | `correct×4` + 8 perfect + 10 whole-scope |
| Tone Hero · Sequence (per game) | `words×4` + 8 perfect + 10 whole-scope |

- **Streak** (`touchDay`): first activity each day; +1 if last active day was yesterday, else resets to 1. Separate from XP.
- XP rewards **effort** (even wrong answers pay).

---

## 4. THE FEATURE TO BUILD — Game achievements feed *Growth*

### 4.1 Concept (LOCKED decisions)
1. **Goal:** let game achievements contribute to the **mascot's GROWTH (`gp`)**, not just XP.
2. **Model A — mastery + bonus:** growth stays anchored to learning; games add on top, **capped**.
3. **Cap = "one stage":** games may contribute **at most ~120–150 growth points total** (≈ a quarter of the 600 climb). **Studying must remain the only path to Master (300) and Sage (600).** This reflects NPK's priority: *promote learning over app usage.*
4. **Two mechanics, sharing the cap budget:**
   - **(a) Per-word clears, weighted by HSK level** (higher level = more points → difficulty variant). Counted **separately per game** (`mm` / `th` / `ts` buckets); a word may pay **once per game** (so matching it in Memory Match *and* clearing it in Tone Hero both count — different skills).
   - **(b) Quantity milestones, per game:** cumulative clear-count rungs **10 / 50 / 100 / 250 / 500 / 1000**, each firing **once**, with escalating bumps. (Per-game, not per-level — avoids unreachable finish lines for small levels.)
5. **Growth from games is permanent** (a clear can't be undone) → it acts as a **rising floor**; the study portion can still ebb/flow above it. Preserves the "buddy reflects what you know" honesty.

### 4.2 Budget allocation (calibration target)
Total game→growth budget ≈ **150** (the cap). Suggested split:
- **~100 pts** from per-word clears (the main, learning-aligned, volume part).
- **~50 pts** from milestones (the satisfying spikes).
- **Hard-cap the combined total at 150** as a safety net regardless of weights.

> ⚠️ Calibrate weights so that **fully clearing all games lands *at* the cap, not far past it** — otherwise the trickle flatlines early and stops feeling responsive. Compute exact constants from the real pool sizes (below). Note the milestone values floated in earlier discussion (e.g. +3/+6/+10/+20/+35/+60) were under a looser scale and **must be rescaled down** to fit the ~50 milestone budget.

### 4.3 Real data for calibration
Per-HSK-level word counts (total 6,100):
| HSK | 1 | 2 | 3 | 4 | 5 | 6 |
|---|---|---|---|---|---|---|
| words | 300 | 200 | 500 | 1000 | 1600 | 2500 |

Game pool sizes: **Memory Match** covers all 6,100; **Tone Hero Sequence (`ts`)** covers the **5,095** words that segment to ≥2 syllables; **Tone Hero Single (`th`)** covers only single-character words (a smaller subset — the build session should count it). So Tone Hero Single will reach fewer milestone rungs than the others — that's fine, it just tops out naturally.

For reference, if a per-clear weight equalled the HSK level number, the weighted full-clear sum for the 6,100-word pool is Σ(count×level) = **29,200** — which across 3 games is ~87,600, ~5× the study pool. That's why per-clear weights must be **small fractions** (e.g. level ÷ N) calibrated down to the ~100-point per-clear budget. The build session should compute N from the actual pools.

### 4.4 Implementation notes (safety-critical)
- **Extend `growthPoints(acct)`** to add the **capped** game contribution. Because growth is a **live recompute** (no stored total), this is **idempotent — no double-count, no data migration**. Existing profiles instantly get retroactive credit for clears already banked (a heavy player's buddy may jump on first open of the updated app — treat as a pleasant "your games count now!" moment).
- **🛑 The one real crash vector:** to weight a cleared id by level you look up `WORD_BY_ID[id]` and read `.grp` (HSK level). If an id is **stale** (word removed/renamed), the lookup is `undefined` and `.grp` **throws** — and `growthPoints` runs in some **unguarded render paths**. **Guard it:** `var w = WORD_BY_ID[id]; if(!w) continue;`. Also default any unknown weight to 0 to avoid `NaN` gp (which would silently drop the buddy to stage 1).
- **Milestones need a fire-once store** (mirror the existing `gameTrophies` pattern): e.g. `a.gameMilestones[game+"|"+rung] = 1`, awarded once. Add a small migration/default like the other maps.
- **Keep displayed growth a clean integer** — compute the fractional game part, then round the final `gp`.
- **STAGES thresholds** (20/60/140/300/600) likely **don't need changing** under a 150 cap, but verify the climb still feels earned.
- **Don't touch** v1/v2 game logic, Memory Match, the XP awards, or the SRS engine. Only: extend `growthPoints`, add milestone tracking, add the guard.

### 4.5 Key code anchors (so the build session doesn't re-hunt)
- `growthPoints(acct)` ≈ line 1310 → `return {gp:m*3+f, mastered:m, familiar:f}`.
- `stageFromGP(gp)` ≈ line 1315; `STAGES` array ≈ 1302.
- `WORD_BY_ID` built ≈ line 1141; `WORD_BY_ID[id]` → word object; `word.grp` = HSK level, `word.ds` = dataset, `word.id` = `ds:group-index`.
- `clearedMap(kind)` ≈ line 2410; buckets `"mm"`, `"th"`, `"ts"`; `markCleared(kind,id)` has first-time guard; `a.cleared = {mm,th,ts}`; migration ≈ line 1348.
- Trophy fire-once pattern: `checkGameCompletion(kind)` ≈ 2417, `showNextTrophy()` — copy this guarding style for milestones.
- `awardXP(n,msg,kind)` ≈ 1394; `touchDay()` ≈ 1399.
- Account schema defaults ≈ line 1330 (`xp, streak, lastDay, days, xpByDay, cards, best, qStats, log`); migrations ≈ 1348–1350.
- Render paths calling `growthPoints` (some **unguarded** — must not throw): progress render ≈ 1789, `awardXP` ≈ 1409, plus game done screens.

### 4.6 Validation before delivering
- `node --check` on **both** inline `<script>` blocks.
- `<div>` open/close balance unchanged.
- Numeric sanity check: simulate full-clear and confirm **game→growth ≤ cap (~150)** and that a study-only path still reaches Master/Sage.
- Spot-check the `WORD_BY_ID` guard with a fake stale id (must not throw).

---

## 5. Roadmap — "2nd class" prestige tier (LATER, not this build)
After a buddy fills growth (reaches Sage) **and passes a special test/quest**, unlock a prestige tier above Sage.
- **Reward = visible transformation** (ascended Bao, new title like "Grandmaster", glow/badge, optional short higher ladder). **Do NOT reset growth to zero** — a shrinking buddy reads as punishment in a *learning* app; **stack** a new tier on top.
- **Gate = a knowledge test games can't shortcut** — ideally a **retention re-test** of words mastered a while ago (proves durable memory, not cramming). This is the deliberate counterweight to letting games nudge growth: *activity helps you climb the hill, but only mastery unlocks the summit.*
- Open questions for then: per-course vs whole-app prestige; how hard the gate; one tier vs a series.

---

## 6. One open question NPK can answer when building starts
The cap is set ("one stage" ≈ 150). The only thing left to feel out during the build is the **per-clear vs milestone split** (suggested ~100 / ~50) and the exact milestone bump values — easy to dial once seen in play.
