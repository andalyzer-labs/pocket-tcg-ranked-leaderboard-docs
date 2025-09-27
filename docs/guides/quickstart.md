# Quick Start Guide – Screenshot Analysis Bot

This guide will help you (the Discord server owner) set up and start using the Screenshot Analysis Bot to track player rankings.

---

## Step 1. Invite the Bot
1. Click the invite link provided by the bot developer.  
2. In the Discord prompt, select the server you want to add the bot to.  
3. You must have **Administrator** permissions.  
4. Authorize the bot with the requested permissions:  
   - Read Messages  
   - Send Messages  
   - Attach Files  
   - Use Slash Commands  

Once authorized, the bot will appear in your server’s member list.

---

## Step 2. Set the Leaderboard Channel
The bot needs to know which channel will be used for leaderboard updates.  

1. Navigate to the channel you want to use for leaderboard posts.  
2. Run the command:  
   ```bash
   /setboard
   ```  
3. The bot will confirm that the leaderboard is now linked to the channel.

---

## Step 3. Set the Current Season
The bot tracks rankings by season. You must define the season before players can start competing.  

1. Run the command:  
   ```bash
   /setseason <season_number>
   ```  
   Example:  
   ```bash
   /setseason A4b
   ```  
2. The bot will confirm the season is active.

---

## Step 4. Start Uploading Screenshots
Players can now upload their rank screenshots in any channel where the bot is active.  

- When a screenshot is posted, the bot analyzes it automatically.  
- It extracts **points, placement, and league tier** (Poké Ball, Great Ball, Ultra Ball, Master Ball).  
- The bot posts results and updates the leaderboard in the channel you configured with `/setboard`.

---

## Step 5. Submit a Screenshot (Players)
Players should use the `/submit` command to upload their ranked battle screenshot. This ensures the bot analyzes and records the result.

### How to Submit
1. Type the command:
   ```bash
   /submit
   ```
2. Attach your screenshot before sending the message.  

Example:
```
/submit
[attach screenshot file]
```

### What Happens Next
- The bot analyzes the screenshot for **points, placement, and league tier**.  
- It confirms the analysis in the channel.  
- The leaderboard in your designated board channel updates automatically.  

### Example Output
```
✅ Screenshot submitted!
Player: @username
Points: 2450
Placement: #5845
League: Master Ball 
```

---

## Step 6. Track the Leaderboard
- The leaderboard channel will update as members upload new screenshots.  
- Everyone can see points, placement, and current season standings.  

---

## Step 7. Resetting for a New Season
When a new competitive season begins, you can reset the leaderboard to start fresh.

1. Run the command:
   ```bash
   /setseason <new_season_number>
   ```
   Example:
   ```bash
   /setseason A4c
   ```
2. The bot will archive the old season’s data and begin tracking the new season.  
3. Announce to your community that the new season has started so players can begin submitting screenshots again.

---

## Useful Commands
- `/help` → Show available commands  
- `/setboard` → Assign the leaderboard channel  
- `/setseason <number>` → Start or switch to a new season  
- `/submit` → Upload a screenshot for analysis  
- `/ping` → Check if the bot is online  

---

🎉 That’s it! Your server is now ready to compete with the Screenshot Analysis Bot. Upload screenshots, track rankings, and enjoy the competition.
