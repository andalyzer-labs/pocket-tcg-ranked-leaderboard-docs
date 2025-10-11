# Slash Command Reference

Use this page to find every slash command exposed by the Pocket TCG Ranked Leaderboard bot, who can run it, and what to expect in response. All commands must be executed from within a Discord server where the bot is installed.

## Quick reference

### Available to everyone

| Command | Arguments | What it does |
| --- | --- | --- |
| /ping | – | Returns an ephemeral “pong” so you can verify the bot is online. |
| /submit | image (screenshot), debug (optional) | OCRs a ranked screenshot, updates the database, and refreshes guild leaderboards. |
| /remove_last | season (optional) | Removes your most recent submission for the chosen (or current) season. |
| /remove_all | confirm (true/false), season (optional) | Deletes all of your submissions for the chosen (or current) season once confirmed. |
| /leaderboard | season (optional), page (number, optional) | Opens an interactive, ephemeral, paginated leaderboard view; page sets the starting page. |
| /me | season (optional) | Displays your latest record plus personal bests for the season. |
| /history | season (optional), limit (number) | Sends an ephemeral list of your recent submissions for the season. |
| /megraph | season (optional), days (number) | Generates time-series charts of your points and placement history. |

### Admin-only commands (requires Manage Server)

| Command | Arguments | What it does |
| --- | --- | --- |
| /admin_setseason | season (text) | Sets the active season code for the server and refreshes any pinned leaderboards. |
| /admin_setboard | – | Creates or refreshes the pinned leaderboard message in the current channel. |
| /admin_set_entry | player (member), points (number), placement (number, optional), season (text, optional) | Manually records a player’s entry for the season, updating the leaderboard. Master Ball entries must include placement. |
| /admin_remove_last | player (member), season (optional) | Removes the player’s most recent submission for the chosen (or current) season. |
| /admin_remove_submission | player (member), submission_id (number), season (optional) | Deletes a specific submission by ID for the player and season. |
| /admin_history | player (member), season (optional), limit (number) | Lists a player’s recent submissions (with IDs) to help identify entries for review. |
| /admin_postboard | interval_hours (number, optional, default 12), season (text, optional) | Immediately posts all leaderboard pages to this channel, deletes previous non‑pinned leaderboard posts by the bot, and schedules the next post every N hours. If season is omitted, each run uses the current season. |
| /admin_stop_postboard | – | Stops the scheduled leaderboard posting for this channel. |

### Season defaults

When a command accepts a season option and you leave it blank, the bot falls back to the server’s current season. Season codes are normalized to uppercase internally. Admin commands that set or infer a season update the stored season for the guild.

## Command details

### /leaderboard
Arguments:
- season (optional): Defaults to the current season.
- page (optional): Starting page (defaults to 1).

Behavior:
- Sends a single ephemeral embed with Prev/Next buttons to navigate pages.
- Browsing does not alter the pinned channel leaderboard’s stored page.
- Display note: placement is only shown for Master Ball entries; Ultra/Great/Poke entries omit placement. When rank text is missing, rank tier is inferred from points.

### /admin_postboard
Permissions: Requires Manage Server.

Arguments:
- interval_hours (number): Hours between posts (default 12; examples: 6, 24, 48).
- season (text, optional): If omitted, each run uses the guild’s current season at post time.

Behavior:
- Immediately deletes prior non‑pinned leaderboard posts authored by the bot in this channel.
- Posts all pages of the current leaderboard to the channel right away.
- Schedules the next posting after interval_hours and continues until stopped with /admin_stop_postboard.

### /admin_stop_postboard
Permissions: Requires Manage Server.

Behavior:
- Stops the scheduled leaderboard posting for the current channel.
