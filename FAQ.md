# ❓ Hangman Game — FAQ

---

## 🛠️ Setup & Configuration

**Q: Where are the config files stored?**

Under your Streamer.bot base directory:
```
Chat Games/Hangman Game/configs.json            ← settings and every chat message
Chat Games/Hangman Game/Hangman-Wordlist.json   ← word pool
Chat Games/Logs/HM-log-YYYY-MM-DD.txt           ← action log, one file per day
```
Older installs also have a `messages.json` next to `configs.json`. Its contents were folded into `configs.json`, and the file is deleted the next time you save. Editing any of this by hand is never necessary — the settings window writes all of it.

---

**Q: The Settings GUI won't open. What do I do?**

If `configs.json` cannot be read at all — another program holds it, or the folder is not writable — the window says so and changes nothing. Close whatever has the file open and try again.

If the file is there but its contents are damaged, the window opens anyway: the damaged file is kept beside it as `configs.json.broken`, and the settings come from `configs.json.bak`, the copy saved before the last write. If that is unusable too, you get the defaults. Either way the window tells you which of the two happened, and Save writes a healthy file again.

Anything else is worth a look in the Streamer.bot log, and worth [reporting](https://github.com/aaskjer/Hangman-Game/issues).

---

**Q: How do I reset all settings to defaults?**

The ↺ button at the bottom left of the settings window. It asks first, then rewrites `configs.json` with factory values and reopens.

---

**Q: I saved settings but nothing changed in-game. Why?**

Save writes the file immediately, and the game reads its config fresh on every command, so the next round already runs on the new values. No restart needed.

---

## 🎮 Starting & Stopping the Game

**Q: Only moderators can start the game — how do I allow everyone?**

Settings → **Access** → turn off **Require Mod To Start**. The broadcaster can always start, whatever this is set to.

**Q: How do I start with Channel Points instead of a chat command?**

1. Turn on **Use Channel Points To Start** on the **Access** page.
2. Paste your **Reward ID** into the field — the button beside it opens a reward lookup.

The chat command then no longer starts a round, but everyone still guesses with `!g` as before.

**Q: How much does a redemption stake?**

The stake is read from the redemption, in this order:

| Where it comes from | When |
|------|-----------|
| What the viewer typed | The reward asks for text and the viewer entered a number |
| The reward's cost | No input, but the reward costs channel points |
| Nothing | Neither applies — the round is free to play |

**Fixed Stake Override** overrules all three. Left at `0` the detection above applies; set to anything higher, every redemption stakes exactly that. Which route was taken is written to the action log on every redemption.

Older setups that used the removed Bet Mode dropdown are migrated on first load: *Fixed* keeps its amount, *Cost*, *Input* and *Free* switch to automatic detection.

**Q: A round is stuck as active after Streamer.bot restarted. How do I clear it?**

It already is clear — the round lives in memory only, so a restart ends it. Just start a new one. During a session, `!hmstop` ends a round.

---

## 💬 Commands & Guessing

**Q: What are the in-chat commands?**

| Command | Who | What it does |
|---------|-----|--------------|
| `!hm` | Mod / Broadcaster | Start a new round |
| `!hmstop` | Mod / Broadcaster | Stop the current round |
| `!g <letter or word>` | Everyone | Guess a letter or the full word |
| `!hmrepeat` | Everyone | Repeat the current word state |
| `!addicted add <user>` | Mod / Broadcaster | Add a user to the addicted list |
| `!addicted remove <user>` | Mod / Broadcaster | Remove a user from the addicted list |

Command names depend on what you configured in Streamer.bot — the above are the defaults.

**Q: How does guessing a full word work? Does it cost lives?**

- **Correct word** → round is won immediately.
- **Wrong word, same length as the hidden word** → costs **2 lives**.
- **Wrong word, shorter than the hidden word** (partial guess) → costs **1 life**.
- Guessing the same wrong word twice sends a duplicate-spam message but does not cost lives again.

**Q: Can the same letter be guessed twice?**

No. A duplicate letter guess sends `MsgAlreadyGuessed` and is ignored — it does not cost lives or count toward the rate limit.

---

## 💰 Points & Currency

**Q: What points system does the game use?**

The game auto-detects your currency in this priority order:
1. **StreamUP Currency Core** — reads `currency_ProductSettings` global.
2. **tawmae Loyalty Points** — reads `tawmae_Settings_LOYALTY POINTS` global.
3. **Config fallback** — uses `PointsVar` / `PointsName` from `configs.json`.

For a custom currency under a different variable name, set **Points Variable** and **Points Name** on the **Access** page.

**Q: How are win points calculated?**

In three steps:

```
base   = RewardWinPoints × (1 + wordBonus%)
stake  = redeemed stake × ChannelPointPayoutMultiplier
paid   = (base + stake) × roleMultiplier
```

`wordBonus%` is `DifficultyBonusPercent` per difficulty level above 1, plus `LengthBonusPercent` per letter beyond `LengthBonusFromLetters`, capped at `MaxWordBonusPercent` — set that cap to `0` to lift it. Turning off **Scale Win By Word** drops the bonus to zero, so every word pays the base.

`roleMultiplier` starts at 1.0 and takes the subscriber boost, the VIP boost and the addicted penalty.

**Q: What does "Reward Per Letter" do?**

Every revealed letter pays **Points Per Letter** on the spot, on top of the win reward that lands when the round ends.

---

## 👥 Role Management

**Q: How do Subscriber / VIP boosts work?**

Turn the boost on under **Game → Role Boosts** and set the percentage. They stack additively:
```
multiplier = 1.0 + (SubBoost%) + (VipBoost%)  →  then clamped to ≥ 0
```
Example: 10% sub boost + 20% VIP boost on a 500-pt win = `500 × 1.30 = 650 pts`.

**Q: What is the "Addicted" system?**

For the regulars who play every round. They keep playing, with two strings attached:
- **Points penalty** — their win multiplier drops by `AddictedPointsPenaltyPercent`, so 50 means they earn half.
- **Guess interval** — they wait `AddictedGuessIntervalSeconds` between their own guesses.

Manage the list with `!addicted add/remove <user>` (mods only) or edit **Addicted Users** on the **Access** page.

---

## 🧠 Hints

**Q: When does the game send a hint?**

When all four hold:
1. `HintEnabled` is on.
2. The word has a hint.
3. Wrong guesses have reached `HintAfterWrongGuesses`.
4. Less of the word is revealed than `HintStopRevealPercent` allows.

The hint fires once per wrong guess check, not repeatedly. Keep `HintAfterWrongGuesses` below your lives — otherwise the round is over before the hint can fire, and the settings window warns you about exactly that on save.

**Q: How do I set hints per word?**

Fill the **Hint** column on the **Words** page. A word without a hint never triggers the hint message, even with hints switched on.

---

## 🛡️ Anti-Cheat & Cooldowns

**Q: What does the rate limit do?**

It caps how many guesses one user may make inside a rolling window. Past `AntiCheatMaxGuesses` within `AntiCheatWindowSeconds` the guess is blocked and `MsgAntiCheat` goes out. Duplicate letter guesses do not count toward it, and broadcasters are exempt. Setting the maximum to `0` switches the limit off.

**Q: What is the difference between Global Cooldown and User Cooldown?**

- **Global Cooldown** — everyone waits before the game can be started again.
- **User Cooldown** — only the person who just started waits.

Both sit on the **Access** page and are written straight into the Streamer.bot command when you save, through `CPH.CommandSetGlobalCooldownDuration` and `CPH.CommandSetUserCooldownDuration`.

---

## 📋 Wordlist

**Q: How do I add or edit words?**

On the **Words** page. Add, delete, search and filter directly in the table; changes stay in memory until you hit Save or Save & Exit.

**Q: What does Difficulty control?**

Each word carries a `Difficulty` of at least 1. Only words within `MinDifficulty … MaxDifficulty` are drawn. Set both to the same number to pin one tier.

**Q: What happens when all words in the difficulty range have been used?**

The used-word pool for that range is cleared and every word becomes eligible again. The log records it as `"All N word(s) in current difficulty range exhausted — resetting used-word pool."`, and the next pick happens right away.

**Q: The wordlist file is missing or corrupt. Will the game break?**

No. Every round start checks `Hangman-Wordlist.json`. Missing or empty, and the built-in default list of 15 words is written to disk — the same list the *Load Defaults* button hands out.

---

## 🏆 Scoreboard

**Q: How is the leaderboard built?**

It is updated whenever a user calls `!hmstats`, not automatically on a win or a loss. A player who never checks their stats never appears on it.

**Q: How do I exclude the broadcaster or bot from the scoreboard?**

On the **Board** page, turn off **Include Broadcaster** or **Include Bot**. **Blocked From Board** takes a comma-separated list for anything else you want hidden, other bots for instance. Hiding someone there does not stop them playing — that list is on the **Access** page.

**Q: How does the monthly auto-reset work?**

Turn on **Reset All (Monthly)** or the per-platform toggles. On the first scoreboard command of a new month, or on and after the configured reset day, that platform's stats are zeroed and the board cleared. A chat message goes out and the date is stored, so it runs once.

---

## 🌐 Multi-Platform

**Q: Which platforms are supported?**

Twitch, YouTube and Kick, all three at once and without a setting to switch them on. The platform comes from the incoming event. To keep the game off one of them, disable the trigger for it in Streamer.bot.

**Q: Can the game run on multiple platforms at the same time?**

Yes. Each platform has its own `GameState` in memory, so a Twitch round and a YouTube round run side by side without touching each other. Points, stats and leaderboards are kept per platform as well.

---

## 📺 OBS Overlay

**Q: What does the overlay show?**

A local HTML file that OBS loads as a browser source and that repaints itself while the round runs: the gallows filling in with every wrong guess, the masked word, and whichever of category, lives, guessed letters, wrong letters, wrong words and hint you switch on. When the round ends it shows the answer and fades out after the delay you set — `0` makes it disappear at once.

**Q: How do I get it into OBS?**

*Setup Overlay in OBS* on the **Overlay** page creates the scene and the browser source over the Streamer.bot OBS connection you already have. By hand works too: the folder button opens the directory, and `hangman_overlay.html` is a plain local file.

**Q: Does the overlay give the hint away?**

No. It follows the same rules as chat — *Use Hints*, *Hint After Wrong Guesses* and *Hints Stop Reveal* — so it never shows a hint chat has not earned. Showing it at all is a separate toggle.

---

## 🔔 Update Notifications

**Q: How do I know when a new version is available?**

Opening the settings window checks GitHub for the latest release in the background. If there is a newer one, a red pill appears in the window header; clicking it opens the releases page. The check never holds the window shut — if GitHub is slow or unreachable, the pill simply does not appear.

---

# Is Hangman Game an AI Slop?
Partially it is. This script has been developed with input from the streamer.bot community and is support by AI.
But i spend a lot of time putting heart and soul in it and my goal was to create a robust and valid moderation tool for everyone and easy to use.
I understand that people, especially IT savvy people, will dislike the project because of the use of AI and i absolutely understand and support their point of view.
But i had a lot of fun making it as with all my other projects, so i used it to "learn" coding and used AI for something valuable. 

AI can create bugs and i am not a developer in classical terms. But i spend a reasonable amount of time fixing any bugs that occured while testing.
If you still find bugs or have something to say, please let me hear it :)
