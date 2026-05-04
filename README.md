# May the 4th — Star Wars Fan Quiz

A self-contained, single-file Star Wars trivia game for the Instacart team.
Choose your side (Jedi or Sith), answer 10 canon-deep questions delivered as
the iconic opening crawl, race the clock, and claim a spot on the leaderboard.

## What's in the box
- Splash → full-screen opening crawl (prelude + zooming logo + scrolling text)
- Login + side selection (Light / Dark — Jedi vs Sith symbols)
- 10 multiple-choice questions across 6 formats: finish-the-quote, "which is
  NOT a SW character / planet / droid / actor", chronological order, lore
- Each question delivered as the angled, scrolling Star Wars text crawl
- Themed SVG art per question (Yoda, Vader, planets, droids, Falcon, etc.)
- Per-question 30-second timer (lightsaber-style ring) with countdown ticks
- Synthesized sound effects via Web Audio API: lightsaber ignite, correct
  chime, wrong buzz, countdown ticks, boom on completion. No audio files,
  no copyright concerns
- Background music via embedded YouTube iframe (configurable video ID)
- Live leaderboard split Light vs Dark with cumulative side totals plus a
  "winning side of the day" banner
- Pure HTML/CSS/JS — no build step, no backend required

## Deploy to GitHub Pages (5 min)
1. Create a new public repo, e.g. `may-the-4th-quiz`
2. Push `index.html` and `README.md` to `main`
3. Repo settings → **Pages** → Source = `Deploy from branch`, Branch = `main` / root
4. Wait ~1 min, then share `https://<user>.github.io/may-the-4th-quiz/`

Works on phones too — perfect for remote participation across the Instacart network.

## Customizing assets

### Swap the music
Edit `MUSIC_VIDEO_ID` near the top of the `<script>` block. Currently set to
`54hoKbTWon4` (the user-supplied opening theme video).

### Add real Star Wars images
The build uses custom SVG art so it works offline and avoids hot-link breakage.
To use real images instead, replace the `art:` field on each question with an
`<img>` tag pointing to your asset URL:

```js
{
  art: '<img src="https://your-cdn/yoda.png" style="height:110px"/>',
  crawl: '...',
  ...
}
```

For an internal-only deployment, good public sources include:
- **Wikimedia Commons** (free-licensed photos, cosplay, props)
- **starwars.com** press/media kits
- Internal Instacart asset libraries

### Tune sound effects
All SFX are generated in JS via the Web Audio API in the `SFX` object. Tweak
frequencies, durations, and waveforms there — no files to manage.

### Edit questions
Change the `QUESTIONS` array. Each entry has `art`, `crawl`, `options[]`,
`correct` (0-indexed), and `note`.

### Timer / scoring
- `TIMER_SECONDS` (default 30)
- Score = `100 + (secondsLeft × 10)` per correct answer

## Leaderboard scope
By default, scores are stored in `localStorage` (per-device). Good for a
shared-screen event or self-tracking. Not good for a true cross-user tally.

### Optional: shared real-time leaderboard
Swap `localStorage` for one of:
- **Firebase Realtime Database** (free, ~10 lines) — recommended
- **Supabase** (free Postgres tier)
- **JSONBin.io** (simplest, public bins)

In `finishQuiz()` and `renderLeaderboard()`, replace the
`localStorage.getItem/setItem('sw_leaderboard', ...)` calls with your API.
Drop-in pseudocode for Firebase:

```js
firebase.database().ref('scores').push(entry);
firebase.database().ref('scores').on('value', snap => {
  const board = Object.values(snap.val() || {});
  // ...rest of renderLeaderboard()
});
```

## Question type ideas (for future expansion)
- "Finish this speech / quote"
- "Put these characters/films in chronological order"
- "Which of these is NOT a Star Wars character / planet / ship / droid?"
- "Which actor has NOT appeared in a Star Wars film?"
- "Match the lightsaber color to its wielder"
- "Which of these is canonically a Force-user?"
