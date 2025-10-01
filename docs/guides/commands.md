# Slash Command Reference

Use this page to find every slash command exposed by the Pocket TCG Ranked Leaderboard bot, who can run it, and what to expect in response. All commands must be executed from within a Discord server where the bot is installed.

## Quick reference

### Available to everyone

| Command | Arguments | What it does |
| --- | --- | --- |
| `/ping` | — | Returns an ephemeral `pong` so you can verify that the bot is online. |
| `/submit` | `image` (screenshot), `debug` (optional) | OCRs a ranked screenshot, updates the database, and refreshes guild leaderboards. |
| `/remove_last` | `season` (optional) | Removes your most recent submission for the chosen (or current) season. |
| `/remove_all` | `confirm` (true/false), `season` (optional) | Deletes all of your submissions for the chosen (or current) season once confirmed. |
| `/leaderboard` | `season` (optional), `page` (number) | Shows an embed with the selected leaderboard page for the season. |
| `/me` | `season` (optional) | Displays your latest record plus personal bests for the season. |
| `/history` | `season` (optional), `limit` (number) | Sends an ephemeral list of your recent submissions for the season (timestamp, rank, placement, points). |
| `/megraph` | `season` (optional), `days` (number) | Generates time-series charts of your points and placement history. |

### Admin-only commands (requires **Manage Server**)

| Command | Arguments | What it does |
| --- | --- | --- |
| `/admin_setseason` | `season` (text) | Sets the active season code for the server and refreshes any pinned leaderboards. |
| `/admin_setboard` | — | Creates or refreshes the pinned leaderboard message in the current channel. |
| `/admin_set_entry` | `player` (member), `points` (number), `placement` (number, optional), `season` (text, optional) | Manually records a player's entry for the season, updating the leaderboard and optionally setting the current season. |
| `/admin_remove_last` | `player` (member), `season` (optional) | Removes the player's most recent submission for the chosen (or current) season. |
| `/admin_remove_submission` | `player` (member), `submission_id` (number), `season` (optional) | Deletes a specific submission by ID for the player and season. |
| `/admin_history` | `player` (member), `season` (optional), `limit` (number) | Lists a player's recent submissions (with IDs) to help identify entries for review. |

### Season defaults

When a command accepts a `season` option and you leave it blank, the bot falls back to the server's current season. Season codes are normalized to uppercase internally. Admin commands that set or infer a season will update the stored season for the guild.

## Command details

### `/ping`
*Response*: Sends `pong 🏓` as an ephemeral reply so only you see the result.

Use it as a quick health check to confirm that the bot is online and responsive.

### `/submit`
*Arguments*:
- `image` (required): Upload a ranked match screenshot (`.png`, `.jpg`, `.jpeg`, `.webp`, `.heic`, or `.heif`).
- `debug` (optional): Set to `true` to receive the raw OCR text back for troubleshooting.

*Behavior*:
- Reads the attachment and OCRs it to extract season, points, placement, rank, and win streak.
- Uses the detected season if present; otherwise falls back to the guild's current season. If neither is available, the command aborts and prompts an admin to run `/admin_setseason`.
- Saves the submission in SQLite and updates the in-memory leaderboard snapshot.
- Sends you an ephemeral recap of your latest stats and refreshes every tracked leaderboard message in the guild.

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

### `/history`
*Arguments*:
- `season` (optional): Defaults to the current season.
- `limit` (number): Defaults to 10, maximum 20.

*Behavior*:
- Fetches your recent submissions for the chosen season.
- Displays them as an ephemeral list including timestamp, inferred rank when necessary, placement, and points.

### `/megraph`
*Arguments*:
- `season` (optional): Defaults to the current season.
- `days` (number): Defaults to 30. Limits the time window for the charts.

*Behavior*:
- Pulls your submission history for the chosen window.
- Generates PNG line charts for points and rank (if enough data exists) and sends them back as ephemeral attachments.

### `/admin_setseason`
*Permissions*: Requires **Manage Server**.

*Arguments*: `season` (text) — supply the new season code (for example, `A3`). The bot stores the season in uppercase automatically.

*Behavior*:
- Updates the stored season for the guild.
- Resets tracked leaderboard pages to page 1.
- Refreshes every tracked leaderboard message in the guild with the new season.

Use this whenever a new season begins or you need to correct the active season.

### `/admin_setboard`
*Permissions*: Requires **Manage Server**.

*Behavior*:
- Validates that the bot can view the channel, send messages, and embed links; it also attempts to pin the leaderboard message (needs **Manage Messages** for automatic pinning).
- Creates the leaderboard message for the current channel if it does not exist, or refreshes it if it does.
- Initializes the channel's leaderboard state to page 1 for the current season.

Run this in the channel where you want the pinned leaderboard embed to live.

### `/admin_set_entry`
*Permissions*: Requires **Manage Server**.

*Arguments*:
- `player` (member): The user whose entry you want to update.
- `points` (number ≥ 0): Ranked points to record.
- `placement` (number ≥ 1, optional): Required for Master Ball players; otherwise optional.
- `season` (text, optional): Defaults to the current season. Providing a value also updates the stored season for the guild.

*Behavior*:
- Normalizes the season and ensures it is tracked as the current season if provided.
- Infers the player's rank from points when possible and enforces that Master Ball entries include a placement.
- Upserts the player's latest record in memory and persists it to SQLite with the current timestamp.
- Attempts to DM the player with the updated stats (skipped for bots, reports if DMs fail).
- Refreshes all tracked leaderboard embeds in the guild.

### `/admin_remove_last`
*Permissions*: Requires **Manage Server**.

*Arguments*:
- `player` (member): Player whose latest submission should be removed.
- `season` (optional): Defaults to the current season.

*Behavior*:
- Deletes the player's most recent submission for the season.
- Rebuilds their latest snapshot from remaining entries and refreshes guild leaderboards.
- Replies with their new latest stats or notes that no entries remain.

### `/admin_remove_submission`
*Permissions*: Requires **Manage Server**.

*Arguments*:
- `player` (member): Player whose submission should be removed.
- `submission_id` (number): ID shown in `/history` or `/admin_history`.
- `season` (optional): Defaults to the current season.

*Behavior*:
- Confirms that the submission ID exists for the player and season.
- Deletes the specific submission, rebuilds the player's latest snapshot, and refreshes guild leaderboards.
- Responds with details of the removed entry and the player's new latest stats (if any).

### `/admin_history`
*Permissions*: Requires **Manage Server**.

*Arguments*:
- `player` (member): Player to inspect.
- `season` (optional): Defaults to the current season.
- `limit` (number): Defaults to 10, maximum 20.

*Behavior*:
- Lists the player's most recent submissions for the season, including submission IDs, timestamps, inferred rank, placement, and points.
- Sends the report ephemerally so admins can identify entries for follow-up actions such as `/admin_remove_submission`.

