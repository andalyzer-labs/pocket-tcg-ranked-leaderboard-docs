# Pocket TCG Ranked Leaderboard â€” Public Release Notes

## Season A4B Â· October 3, 2025 (v0.1.0)
- Leaderboard embeds now display players grouped by rank tier with clear section headers (Master â†’ Ultra â†’ Great â†’ Poke â†’ Beginner â†’ Unranked).
- Master Ball entries older than 24 hours automatically show a strikethrough on their placement so communities can spot stale rankings at a glance.
- Embed accent color updated to the new signature purple (`#8440B7`) to match current branding.
## Season A4B — 2025-10-11 (v0.2.0)
- New ephemeral, paginated /leaderboard view with Prev/Next buttons. Browsing is private and does not change the pinned board’s stored page.
- Placement is now shown only for Master Ball entries; Ultra/Great/Poke entries omit placement to reduce confusion.
- Rank tier details are displayed for non-Master groups when available (e.g., “Ultra Ball Rank 2”); when rank text is missing, the tier is inferred from points.
- Admin: /admin_postboard immediately posts all leaderboard pages in-channel, deletes prior non-pinned leaderboard posts by the bot, and schedules the next post every N hours (default 12). /admin_stop_postboard stops scheduled posts.
- Docs refreshed to reflect the above behaviors.
