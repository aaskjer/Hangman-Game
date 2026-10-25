# ❓ Hangman Game — FAQ

---

## 🛠️ Setup & Configuration

**Q: Where are the config files stored?**

Under your Streamer.bot base directory:
```
Chat Games/Hangman Game/configs.json            ← settings and every chat message
Chat Games/Hangman Game/Hangman-Wordlist.json   ← word pool
Chat Games/Logs/HM-log-YYYY-MM-DD.txt           ← action log, one file per day
Chat Games/Hangman Game/Import Cache/           ← OdeNet download for the word importer
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

It already is clear — the round lives in memory only, so a restart ends it. Just start a new one. During a session, `!killhm` ends a round.

---

## 💬 Commands & Guessing

**Q: What are the in-chat commands?**

| Command | Who | What it does |
|---------|-----|--------------|
| `!hm` / `!hangman` | Mod / Broadcaster | Start a new round |
| `!hm blitz` | Mod / Broadcaster | Start a [blitz round](#-blitz-rounds) |
| `!killhm` / `!killhangman` | Mod / Broadcaster | Stop the current round |
| `!g` / `!guess` `<letter or word>` | Everyone | Guess a letter or the full word |
| `!hmr` / `!hangmanrepeat` | Everyone | Repeat the current word state |
| `!hmp` / `!hangmanpersonal` | Everyone | Your own stats, with your streak |
| `!hml` / `!hmlookup` `<user>` | Everyone | Someone else's stats |
| `!hms` / `!hangmanscoreboard` | Everyone | The scoreboard for this platform |
| `!hma` / `!hangmanaddict` `add <user>` | Mod / Broadcaster | Add a user to the addicted list |
| `!hma` / `!hangmanaddict` `remove <user>` | Mod / Broadcaster | Remove a user from the addicted list |

These are the commands as they come in the import. You can rename them in Streamer.bot's `Commands` tab.

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
base   = RewardWinPoints × (1 + wordBonus%) × blitz × (1 + streakBonus%)
stake  = redeemed stake × ChannelPointPayoutMultiplier
paid   = (base + stake) × roleMultiplier
```

`wordBonus%` is `DifficultyBonusPercent` per difficulty level above 1, plus `LengthBonusPercent` per letter beyond `LengthBonusFromLetters`, capped at `MaxWordBonusPercent` — set that cap to `0` to lift it. Turning off **Scale Win By Word** drops the bonus to zero, so every word pays the base.

`blitz` is the **Blitz Payout Multiplier** in a [blitz round](#-blitz-rounds) and 1 otherwise. `streakBonus%` comes from the winner's [streak](#-streaks) and is 0 while streaks are off.

`roleMultiplier` starts at 1.0 and takes the subscriber boost, the VIP boost and the addicted penalty.

**Q: What does "Reward Per Letter" do?**

Every revealed letter pays **Points Per Letter** on the spot, on top of the win reward that lands when the round ends. In a blitz round it is multiplied too.

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

Manage the list with `!hma add/remove <user>` (mods only) or edit **Addicted Users** on the **Access** page.

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

## ⚡ Blitz Rounds

**Q: What is a blitz round?**

A short, sharp round. It runs on **Blitz Duration** and **Blitz Lives** instead of the normal round settings, and every point it pays — the letter rewards and the win — is multiplied by **Blitz Payout Multiplier**. The channel point stake is not multiplied. The overlay shows a pulsing ⚡ BLITZ badge while it runs.

**Q: How does a round become a blitz?**

Turn on **Enable Blitz** under **Game → Blitz Round**. Every round started from then on, by chat command or channel points, has **Blitz Chance** percent to be a blitz, and moderators can call one with `!hm blitz` while **Allow '!hangman blitz'** is on. Set the chance to `0` to have blitz rounds only on command.

---

## 👾 Evil Hangman

**Q: What does Evil Hangman do?**

Once chat has used up **Glitch At** percent of the round's lives — 50 with 6 lives fires on the 3rd wrong guess — the overlay glitches and the word is swapped for another. Every guess is wiped except one randomly picked letter that was already revealed, and the new word always contains it. It happens at most once per round, and **Evil Chance** decides how many rounds are evil at all.

**Q: Do lives refill after the swap?**

Your choice. **Refill Lives On Swap** gives the new word full lives; with it off, lives already lost stay lost.

**Q: Which word does it swap to?**

One from the same category when **Prefer Same Category** is on and a word fits, otherwise from any category in play — always within your difficulty range and always containing the surviving letter. If no other word contains that letter, nothing survives.

**Q: Why is chat quiet while the overlay glitches?**

The swap message waits until the glitch has played out on the overlay (**Glitch Duration** plus half a second), and guesses, `!hmr` and the "already running" reply are ignored until then — any reply would show the new word before the overlay does. If the round ends during the glitch, the message is dropped. With the duration at `0`, or the overlay switched off, the swap is announced at once.

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

## 📥 Importing Words

**Q: Where can the importer get words from?**

*Import* on the **Words** page fetches from four free sources:

| Source | What | Language | License |
|--------|------|----------|---------|
| [Wikidata](https://www.wikidata.org) | Countries, capitals, big cities, rivers, mountains, chemical elements, musical instruments, sports, dog breeds — or any Wikidata class by its Q-ID | German, English | CC0 |
| [OdeNet](https://github.com/hdaSprachtechnologie/odenet) | German nouns by topic — animals, plants, food, objects, people & jobs, body, places, materials, nature, feelings — with their meaning as the hint | German | CC BY-SA 4.0 |
| [PokéAPI](https://pokeapi.co) | Pokémon by generation, with their category as the hint | German, English | — |
| [IGDB](https://www.igdb.com) | Video games, best-rated first, with year and genre as the hint | English titles | IGDB terms |

You see the list before anything is added: untick what you don't want, fix a hint in the table, and the rest lands in the category you named. Nothing is kept until you click Save.

**Q: What does the importer leave out?**

Answers with digits or symbols, more letters than **Max. Letters** or more words than **Max. Words Per Answer**, and words already in your list. Accents are folded (é → e). Umlauts stay letters to guess, unless **Write umlauts out** turns them into ae, oe, ue and ss. A hint that names its own answer is swapped for another one or masked with `___`.

**Q: How is the difficulty picked?**

*Auto* rates every word from its length, rare letters (j, q, x, y, z) and how well known it is — the rank in the result for Wikidata and IGDB, the generation for Pokémon. Or pick one fixed difficulty for the whole import.

**Q: What does IGDB need?**

The Client ID and Client Secret of a free Twitch developer app. Create one at [dev.twitch.tv/console/apps](https://dev.twitch.tv/console/apps) with any name, `http://localhost` as the redirect URL and *Confidential* as the client type. Both values are kept in Streamer.bot's global variables (`hm_IgdbClientId`, `hm_IgdbClientSecret`), never in `configs.json`.

**Q: Some OdeNet hints don't match the word.**

OdeNet groups synonyms, and a few groups mix two meanings. The importer picks the word whose meanings fit the topic best, but read the list before you add it. The OdeNet file (about 29 MB) is downloaded once and kept in `Chat Games/Hangman Game/Import Cache` for a month.

---

## 🏆 Scoreboard

**Q: How is the leaderboard built?**

From the players' stats, read fresh every time someone asks with `!hms`. Everyone who has won at least one round on that platform is on it, ranked by wins and then by points.

**Q: How do I exclude the broadcaster or bot from the scoreboard?**

On the **Board** page, turn off **Include Broadcaster** or **Include Bot**. **Blocked From Board** takes a comma-separated list for anything else you want hidden, other bots for instance. Hiding someone there does not stop them playing — that list is on the **Access** page.

**Q: How does the monthly auto-reset work?**

Turn on **Reset All (Monthly)** or the per-platform toggles. On the first scoreboard command of a new month, or on and after the configured reset day, that platform's stats are zeroed and the board cleared. A chat message goes out and the date is stored, so it runs once.

---

## 🔥 Streaks

**Q: How do streaks work?**

Turn on **Track Streaks** on the **Board** page. Every word a player solves adds one to their streak. It only breaks when a round they guessed in is lost — game over or time up. A round a moderator stops breaks nothing.

**Q: Do streaks pay?**

**Streak Bonus** adds a percentage to the win for every win in a row after the first — 10 means the third win in a row pays +20 % — up to **Max. Streak Bonus**. Streaks from **Announce From** on are cheered after a win and called out when they end.

**Q: Where do players see their streak?**

`!hmp` adds a line with the current and the best streak. `{streak}` and `{beststreak}` also work in the board and stats messages. The monthly reset clears streaks along with the rest of the stats.

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

**Q: What does the overlay show when a round ends?**

| Ending | Banner |
|--------|--------|
| Solved | `SOLVED - <word>` in green |
| Last life gone | `GAME OVER - <word>` in red |
| Time up | `TIME UP - <word>` in red |
| `!killhm` | `GAME OVER - MODERATOR STOPPED` in orange |

The answer is spelled out above the banner either way.

**Q: After an update the overlay looks the same. Why?**

OBS keeps running the page it loaded, even when the file on disk is replaced. The game therefore refreshes every browser source that shows `hangman_overlay.html` by itself — after an update and once per Streamer.bot session — over the OBS connection set on the **Overlay** page. If OBS is not connected through Streamer.bot, press **Refresh Overlay in OBS**, or right-click the browser source in OBS and choose *Refresh*. From then on the page also reloads itself whenever a newer version lands on disk.

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
