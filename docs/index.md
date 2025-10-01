# Pocket TCG Ranked Leaderboard

![Pocket TCG Ranked Leaderboard logo](logo1.png)

Welcome to the home of the Pocket TCG Ranked Leaderboard bot. These docs walk server owners, moderators, and competitive players through setup, day-to-day use, and advanced tooling so you can keep your community's ranked grind organized.

## What the bot does

- Captures ranked screenshots submitted with , extracts stats via OCR, and updates a live leaderboard for each Discord server.
- Pins a paginated leaderboard embed with the latest entry per player, auto-refreshing after every submission.
- Tracks personal bests, graphs progress over time, and keeps per-season history in SQLite so data survives restarts.
- Supports multiple languages (English, Japanese, Korean, Simplified & Traditional Chinese) out of the box.

## Quick start

1. Follow the [Quickstart guide](quickstart.md) to deploy the bot, configure OCR, and invite it to your server.
2. Use  in the channel where you want the pinned leaderboard and  to define the current season code.
3. Encourage players to upload ranked results with . They can review their stats with  and  or undo mistakes using .

## Learn more

- Review the [Commands reference](commands.md) for every slash command and permission note.
- Check the [FAQ](FAQ.md) for troubleshooting tips, privacy questions, and common workflow answers.
- Visit the [Admin guides](admin/) and [Legal section](legal/) when you need deep dives on hosting, billing, privacy, or terms of service.

If you get stuck, open an issue on [GitHub](https://github.com/andalyzer-labs/pocket-tcg-ranked-leaderboard) and we'll help you get back on track.
