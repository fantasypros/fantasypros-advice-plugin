---
name: injury-check
description: Check an NFL player's current injury status, practice report, and latest news with FantasyPros data. Use when the user asks whether a player is playing, what a player's injury status is, the latest on an injury, or whether a player is active.
---
# Injury check

Workflow for "is X playing?" and "what's the latest on X?". Answers come from the current FantasyPros injury report and news first; the web only fills gaps, and only with this season's dated sources.

**Inputs:** the player(s) the user asks about.

1. **Look them up.** You MUST call `injury_status` with every player in one call.
   - If the user has a league, pass its `league_key` and each name as the user typed it, a bare surname included ("warren"). The tool resolves it against the league's rosters, the user's own team first, and reports who each name became in `name_corrections`. Never swap in the most famous player with that surname yourself.
   - Without a league, use full first and last names (expand shorthand like "CMC" from context). If the result has `did_you_mean`, retry with the player the conversation points to, or ask.
   - Check `resolved_players` before answering: if it shows a different player than the user meant, or `ambiguous_names` is present, re-run with the right `player_id` and say which player you answered about.
2. **Report what FantasyPros has.**
   - `practice_details` is the current injury report: give the designation and the practice participation it shows.
   - `player_news` is FantasyPros news from the last 14 days, newest first. Summarize the newest relevant item and you MUST say when it was published (convert the UTC `published` time to a plain date, e.g. "Friday, Oct 9"). Link its `url` when you cite it.
3. **When FantasyPros has nothing.** No practice information and no news means only that the injury feed has nothing on this player — it does NOT mean he is healthy, and it does NOT mean he is inactive.
   - You MUST call `get_ecr` with `ranking_type="ROS"` and the player's position (DL, LB, or DB for defenders) to check that experts rank him as a current player.
   - Then you MAY search the web for his latest news: prefer fantasypros.com, use only this season's reports, and take the newest. Give the source and its date. If you find nothing current, say you do not have current information on him. Never answer from memory.
4. **Fantasy impact.** If the player is out, doubtful, or questionable:
   - You SHOULD call `get_nfl_depth_charts` for his team and name who is next in line at his position.
   - If he is on the user's roster and the user is setting a lineup, hand off to the start-sit workflow for the replacement; for a pickup, the waiver-wire workflow.
5. **Answer.** Lead with one status line — designation (or "no injury designation"), whether he is expected to play, and the date of the report or news it rests on. Then the fantasy impact in a sentence or two.

## Data sources
- Injury designations, practice reports, and FantasyPros news MUST come from the tools above and take precedence over anything on the web. Never let an older web report override a newer FantasyPros one.
- Results carry `data_as_of` (date, season, week, and that week's `nfl_week_dates`; "this week" is that week); news items carry `published`. Always tell the user how recent the information is.
- Web search is for gaps only: prefer fantasypros.com and the most recent sources, skip anything from a previous season, and say where the information came from and when it was published.
