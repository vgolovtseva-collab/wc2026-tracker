# 🏆 World Cup 2026 · Tracker & Predictions — Quick Guide

**Open it here:** https://vgolovtseva-collab.github.io/wc2026-tracker/

It's a single-page app, nothing to install. Works on phone and desktop. All your data stays in your own browser — everyone on the team has their own independent copy of the tournament.

## How to use it

**Matches tab** — the heart of the app. All 104 matches with real dates, venues and kick-off times (shown in your local time zone).
- For every match that hasn't been played yet you'll see a prediction: Home / Draw / Away probabilities with a colored bar, plus the most likely score.
- When a match finishes, click **⟳ Sync results from web** to pull real scores automatically — or just type the score into the two boxes yourself. Both do exactly the same thing (sync simply overwrites manual entries once the official score is in the feed).
- For a knockout match that ends in a draw, pick the penalty shoot-out winner with the buttons that appear under the match.
- Use the stage filter and the "unplayed only" checkbox to navigate.

**Groups tab** — live tables of all 12 groups, recalculated on every result. Green = direct qualification (top 2), blue = current 3rd place (the 8 best third-placed teams also advance).

**Knockout tab** — the official FIFA bracket from the Round of 32 to the final. It fills in automatically as group standings are decided, including the tricky allocation of third-placed teams to their bracket slots.

**Team chances tab** — the fun one. Press **▶ Recalculate** and the app simulates the *entire remaining tournament* thousands of times, then shows every team's probability of reaching the Round of 32, Round of 16, quarter-finals, semis, the final, and winning the whole thing.

**Ratings tab** — each team's Elo strength rating. The starting values are editable, so if you think the model underrates someone, change the number and all predictions update instantly.

**Reset all** — wipes your entered results and rating edits if you want a clean slate.

## How the predictions actually work

The model has three layers:

1. **Elo ratings.** Every team starts with a strength rating (roughly matching eloratings.net at the start of the tournament). After every result you enter, ratings update: beating a strong team earns a lot of points, beating a weak one earns few, and bigger winning margins move ratings more (K=50 with a goal-difference multiplier). Mexico, the USA and Canada get a +80 home bonus when playing in their own country. **This is how the app "learns" from results** — a team on a surprise run climbs the ratings, and its future predictions improve match by match.

2. **Poisson score model.** For any single match, the Elo gap between the two teams is converted into expected goals for each side (a stronger team is expected to score more, a weaker one less). A Poisson distribution over all possible scorelines then gives the Home/Draw/Away percentages and the most likely score you see on each match card.

3. **Monte Carlo simulation.** For the "Team chances" tab, the app plays out the whole remaining tournament 2,000–10,000 times: every unplayed match is randomly "rolled" from its Poisson distribution, group tables are computed with FIFA tiebreakers, third-placed teams are slotted into the real bracket, knockout draws are decided as shoot-outs, and Elo ratings keep updating *inside* each simulation. Counting how often each team reaches each stage across all those simulated tournaments gives the probabilities.

Important caveat: matches you've already entered are treated as facts — only the future is simulated. So the more results you (or the sync button) feed in, the sharper the forecasts get.

## Good to know

- **Your data is local.** Results and rating edits live in your browser's localStorage. Different device or browser = a fresh start. The shared link never exposes anyone's data.
- **Sync needs internet** and occasionally a public proxy is down — if it fails, the app tells you and you can paste the feed JSON manually or just type scores in.
- **Penalty winners aren't in the feed**, so after syncing a drawn knockout match, pick who went through by hand.
- It's a model, not a crystal ball 🙂 — probabilities are calibrated guesses based on team strength, not inside information about lineups or injuries.
