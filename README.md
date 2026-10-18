# RimRunnerSouthDakota

Unofficial South Dakota HS boys basketball ratings. Not affiliated with the SDHSAA or MaxPreps.
Status: **HELD (waiting on hosting)**. Preview: https://joshua-kpg.github.io/RimRunnerSouthDakota/ (GitHub Pages). No Firebase project yet
(the Google accounts are at their project quota) and Cloudflare Pages is at its project limit, so accounts, picks and follows save only in the
visitor's browser until the site's own Firebase project exists.

## Files
- `index.html` — the whole app (data embedded). `logos/` — small WebP school logos (from MaxPreps school profiles), served with the site.
- `data/results.json` — 2026-27 finals ledger written by the nightly updater.
- `firebase/` — Firestore rules and auth config for the site's future Firebase project (accounts, picks, follows).

## Teams
155 teams. 154 are the SDHSAA 2026-27 boys basketball alignment (SDHSAA Classification & Alignments 2026-27 & 2027-28, which links the
official boys basketball alignment on GoBound): Class AA 20, Class A 52, Class B 82. Class A and B are split into SDHSAA regions
(Region 1A-8A, 1B-8B), shown as the conferences; Class AA has no regions, so its 20 teams form one group, "Class AA".
The SDHSAA Basketball handbook lists no 2026-27 changes.
Other (1): Tri-State (Rosholt/Fairmount/Campbell-Tintah co-op, a North Dakota NDHSAA program filed on MaxPreps under Rosholt, SD). It played
8 games against SDHSAA teams in 2025-26 and has a 2026-27 schedule, so it meets the Other rule.
Co-op/duplicate MaxPreps pages fold into the listed team (`mpAliasIds`): Parker/Marion, Arlington, Lemmon, Miller, Sanborn Central/Woonsocket,
Wilmot.

## Schedule
2026-27 games come from MaxPreps team schedules plus the official SDHSAA schedules on GoBound (the SDHSAA requires schools to post their
schedules there). A GoBound game between two listed teams is added only when MaxPreps has no game for that pair within 3 days. Where the two
disagree, the official SDHSAA (GoBound) date, time and home team are used, and a second MaxPreps listing within 3 days of a game the SDHSAA
lists once is dropped as a copy.

## Season rule
SDHSAA 2026-27 boys basketball: first practice Mon Nov 30, 2026; first allowable contest Thu Dec 10, 2026 (all classes); Winter Moratorium
Dec 23-26, 2026 (no contests); last regular-season contest Fri Feb 26, 2027 (A, B) / Tue Mar 2 (AA); state tournaments Mar 18-20, 2027.
Games involving an SDHSAA team before Dec 10, 2026 or during the moratorium are left out (schedule build and nightly updater); games between
two non-SDHSAA schools are kept. Duplicate next-day listings of one game are removed (shared tournament-repeat rule, tested on both seasons).

## Times
The site clock is Central (America/Chicago). Game times are the host school's local time, as both MaxPreps and GoBound post them; games
hosted by a Mountain-time (West River) school carry `tz: "MT"` and are placed on the clock with the Mountain offset (America/Denver).

## Nightly scores
`.github/workflows/nightly-scores.yml` runs `scripts/update_scores.py` at 07:30 and 15:00 UTC (1:30 am / 9:00 am Central standard time) and
on manual dispatch. Before `SEASON_START` in index.html (Thu Dec 10, 2026) the script makes no requests. From then on it reads boys basketball
finals for the last 3 days from the MaxPreps South Dakota scores-by-date page (team-page fallback), matches them to the schedule by MaxPreps
team id + date (co-op pages by `mpAliasIds`; games from the GoBound fill, which have no MaxPreps contest id, by team pair + date), records them
in `data/results.json` and bakes scores and W-L records into `index.html`. The page recomputes Elo from those finals. It commits only when
data changed; GitHub Pages updates from the commit. The Cloudflare step is skipped while the site is HELD (no API token secret).
