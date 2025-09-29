# Slash Command Reference

Use this page to find every slash command exposed by the Pocket TCG Ranked Leaderboard bot, who can run it, and what to expect in response. All commands must be executed from within a Discord server where the bot is installed.

## Quick reference

| Command | Arguments | Who can use it | What it does |
| --- | --- | --- | --- |
| `/ping` | — | Anyone | Returns an ephemeral `pong` so you can verify that the bot is online. |
| `/setseason` | `season` (text) | Admins with **Manage Server** | Sets the active season code for the server and refreshes any pinned leaderboards. |
| `/setboard` | — | Admins with **Manage Server** | Creates or refreshes the pinned leaderboard message in the current channel. |
| `/submit` | `image` (screenshot), `debug` (optional) | Anyone | OCRs a ranked screenshot, updates the database, and refreshes guild leaderboards. |
| `/history ` | `season` (optional) | Anyone | Sends an ephemeral list of your most recent submissions for the season (timestamp, rank, placement, points), inferring rank from points when the stored rank is missing |
| `/remove_last` | `season` (optional) | Anyone | Removes your most recent submission for the chosen (or current) season. |
| `/remove_all` | `confirm` (true/false), `season` (optional) | Anyone | Deletes all of your submissions for the chosen (or current) season once confirmed. |
| `/leaderboard` | `season` (optional), `page` (number) | Anyone | Shows an embed with the selected leaderboard page for the season. |
| `/me` | `season` (optional) | Anyone | Displays your latest record plus personal bests for the season. |
| `/megraph` | `season` (optional), `days` (number) | Anyone | Generates time-series charts of your points and placement history. |
| `/diag` | — | Anyone | Prints version information for OCR dependencies (useful for troubleshooting). |

### Season defaults

When a command accepts a `season` option and you leave it blank, the bot falls back to the server's current season. Season codes are normalized to uppercase internally.

## Command details

### `/ping`
*Response*: Sends `pong 🏓` as an ephemeral reply so only you see the result.

Use it as a quick health check to confirm that the bot is online and responsive.

### `/setseason`
*Permissions*: Requires the **Manage Server** permission.

*Arguments*: `season` — supply the new season code (for example, `A3`). The bot stores the season in uppercase automatically.

*Behavior*:
- Updates the stored season for the guild.
- Resets tracked leaderboard pages to page 1.
- Refreshes every tracked leaderboard message in the guild with the new season.

Use this whenever a new season begins or you need to correct the active season.

### `/setboard`
*Permissions*: Requires the **Manage Server** permission.

*Behavior*:
- Validates that the bot can view the channel, send messages, and embed links; it also attempts to pin the leaderboard message (needs **Manage Messages** for automatic pinning).
- Creates the leaderboard message for the current channel if it does not exist, or refreshes it if it does.
- Initializes the channel's leaderboard state to page 1 for the current season.

Run this in the channel where you want the pinned leaderboard embed to live.

### `/submit`
*Arguments*:
- `image` (required): Upload a ranked match screenshot (`.png`, `.jpg`, `.jpeg`, `.webp`, `.heic`, or `.heif`).
- `debug` (optional): Set to `true` to receive the raw OCR text back for troubleshooting.

*Behavior*:
- Reads the attachment and OCRs it to extract season, points, placement, rank, and win streak.
- Uses the detected season if present; otherwise falls back to the guild's current season. If neither is available, the command aborts and prompts an admin to set the season.
- Saves the submission in SQLite and updates the in-memory leaderboard snapshot.
- Sends you a recap of your latest stats and refreshes every tracked leaderboard message in the guild.

### `/history`

Arguments:
- `season` (optional): Defaults to the current season.

Behavior:
- Fetches your recent submissions for the chosen (or current) season.  
- Displays them as an ephemeral list including timestamp, rank, placement, and points.  
- If the stored rank is missing, the bot infers the rank from your points.

Use this to quickly review your personal match history for the season.


### `/remove_last`
*Arguments*: `season` (optional) — defaults to the current season.

*Behavior*:
- Deletes your most recent submission for the selected season.
- Rebuilds your "latest" leaderboard entry from the remaining submissions (if any) and refreshes all guild leaderboards.
- Replies with your new latest stats or a notice that no entries remain.

### `/remove_all`
*Arguments*:
- `confirm` (boolean): Must be set to `true` to actually delete anything. If omitted or false, the command responds with a safety warning.
- `season` (optional): Defaults to the current season.

*Behavior*:
- Deletes every submission you have for the chosen season once confirmed.
- Refreshes your leaderboard entry and all guild leaderboards.
- Replies with the count of deleted submissions.

### `/leaderboard`
*Arguments*:
- `season` (optional): Defaults to the current season.
- `page` (number): Defaults to 1. Each page shows the next set of players.

*Behavior*: Returns an ephemeral embed containing the requested leaderboard page, including total players and pagination info.

### `/me`
*Arguments*: `season` (optional) — defaults to the current season.

*Behavior*: Replies with your latest submission stats (points, placement, rank) and any recorded personal bests for the season.

### `/megraph`
*Arguments*:
- `season` (optional): Defaults to the current season.
- `days` (number): Defaults to 30. Limits the time window for the charts.

*Behavior*:
- Pulls your submission history for the chosen window.
- Generates PNG line charts for points and rank (if enough data exists) and sends them back as ephemeral attachments.

### `/diag`
*Response*: Prints Python, Pillow, pytesseract, and system Tesseract versions, or returns the exception raised during checks.

Use this to verify that OCR dependencies are installed correctly when debugging deployment issues.
