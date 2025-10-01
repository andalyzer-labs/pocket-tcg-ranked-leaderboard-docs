# Pocket TCG Ranked Leaderboard FAQ

This FAQ is aimed at Discord server owners, moderators, and players who are interested in running or using the Pocket TCG Ranked Leaderboard bot.

## General

**Q: What does the bot do?**  
A: The bot keeps a live, ranked leaderboard for Pokémon Trading Card Game (Pocket) ranked mode. Players post a screenshot of their in-game rank using <code>/submit</code>, the bot extracts their season, points, placement, and rank via OCR, and then refreshes the pinned leaderboard embed for that server.

**Q: Which communities can use it?**  
A: The bot is guild-scoped, so every server gets an isolated leaderboard, season tracker, and database entries. It is safe to run in multiple servers at once without leaking data between them.

**Q: Where can I find policies or long-form documentation?**  
A: Terms of Service, the Privacy Policy, admin guides, and other docs live at the project documentation site: <https://andalyzer-labs.github.io/pocket-tcg-ranked-leaderboard-docs/>.

## Server Owner & Mod Workflow

**Q: How do I set the active season for my server?**  
A: Run <code>/admin_setseason</code> (e.g., /admin_setseason A4A). The bot records the season per guild and reapplies it whenever new submissions arrive. If players upload a screenshot that shows a different season code, the bot automatically adopts the detected value and uses it going forward.

**Q: How do I create the pinned leaderboard message?**  
A: In the channel where you want the board, run <code>/admin_setboard</code>. The bot checks permissions, creates (or refreshes) the pinned embed, and saves the board state for that channel. If it cannot pin automatically, you can pin the message manually afterward.

**Q: Can I view the leaderboard without the pinned message?**  
A: Yes. Use <code>/leaderboard</code> to get an ephemeral embed of the latest standings for any season and page. The pinned message stays in sync, but the command is handy for quick lookups.

## Player Workflow

**Q: How do players submit their rank?**  
A: Run <code>/submit</code> and attach a PNG, JPG, WebP, or HEIC/HEIF screenshot of your in-game ranked screen. The bot validates the attachment, runs OCR, and updates both the SQLite record and the pinned leaderboard.

**Q: What if OCR misreads my screenshot?**  
A: Try a clearer capture (cropped to the results screen) and ensure both points and placement are visible. You can also pass debug=true to <code>/submit</code> to receive the raw OCR text as ocr.txt, which helps diagnose formatting issues. Persistent failures usually mean Tesseract is missing language packs or the screenshot quality is too low.

**Q: How can I see my personal bests?**  
A: Use <code>/me</code> to view your latest submission plus season personal bests (highest points and best placement). For a time-series graph of points and placement history, run <code>/megraph</code> (optionally with days=<N>).

**Q: Can I undo a mistaken submission?**  
A: Yes. Use <code>/remove_last</code> to delete your most recent submission for the chosen (or current) season. To wipe every submission you have in that season, re-run <code>/remove_all</code> with <code>confirm=true</code>.

## Leaderboard Behavior

**Q: How is the leaderboard ordered?**  
A: The pinned embed shows the most recent submission per player for the selected season, sorted by points (descending) with placement and inferred rank shown for context. Pagination buttons let you move through the board twenty players at a time.

**Q: How fast do updates appear?**  
A: As soon as a submission is processed, the bot refreshes every tracked leaderboard message in that guild. It also keeps track of the last viewed page per channel so users remain on the same page after updates.

**Q: Does the bot infer ranks when the screenshot only shows points?**  
A: Yes. If the OCR did not capture a rank label, the bot derives the expected rank tier from your points using the built-in threshold table that mirrors the in-game progression.

**Q: Which game languages are supported?**  
A: The OCR pipeline searches for season, points, and placement keywords across English, Japanese, Korean, and both Simplified and Traditional Chinese. You can trim languages by editing the TESS_LANG constant if you deploy a custom build.

## Privacy & Control

**Q: What personal information is stored?**  
A: Only the Discord user ID, display name, guild ID, season code, points, placement, rank (if detected), win streak (reserved), and timestamps. No message content or screenshots are saved, and there is no cross-server aggregation.

**Q: How can a player remove their data?**  
A: <code>/remove_last</code> and <code>/remove_all</code> let players manage their own entries. Server admins can also archive or delete the SQLite file if they need a full reset.

**Q: Who can run admin commands?**  
A: Commands such as <code>/admin_setseason</code> and <code>/admin_setboard</code> are restricted to members with the **Manage Guild** permission. All other commands work for regular server members in channels where the bot can respond.

For anything not covered here, visit the documentation site or open an issue on the GitHub repository.
