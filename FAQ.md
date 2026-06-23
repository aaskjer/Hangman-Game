# ❓ Hangman Game — FAQ

---

## 🛠️ Setup & Configuration

**Q: Where are the config files stored?**

All config files live under your Streamer.bot base directory:
```
Chat Games/Hangman Game/configs.json       ← game settings
Chat Games/Hangman Game/messages.json      ← all chat messages
Chat Games/Hangman Game/Hangman-Wordlist.json ← word pool
Chat Games/Logs/HM-log.txt                ← action log
```
You should never need to edit these by hand — use the built-in Settings GUI instead.

---

**Q: The Settings GUI won't open / crashes immediately. What do I do?**

- Check the Streamer.bot log for error details and please report it [here](https://github.com/aaskjer/Hangman-Game/issues).
- If `configs.json` or `messages.json` is malformed, the GUI will fail to open. Delete the corrupt file and restart — it will be recreated with defaults.

---

**Q: How do I reset all settings to defaults?**

Open the Settings GUI and click **Reset to Defaults** (red button, bottom-left). This rewrites both `configs.json` and `messages.json` with factory values and reopens the GUI.

---

**Q: I saved settings but nothing changed in-game. Why?**

Clicking **Save** writes the JSON files to disk. The game script reads config fresh on every `Execute()` call, so the next game command will pick up your changes immediately. No Streamer.bot restart needed.

---

## 🎮 Starting & Stopping the Game

**Q: Only moderators can start the game — how do I allow everyone?**

In the Settings GUI → **Game** tab, uncheck **Require Mod To Start**. Broadcasters can always start regardless of this setting.

**Q: How do I start with Channel Points instead of a chat command?**

1. Enable **Allow Channel Points To Start** in the Settings GUI → **Other Tabs → Twitch Channel Points**.
2. Paste your **Reward ID** into the field (use the ❓ button to look it up).
3. Set your preferred **Bet Mode** (`fixed`, `cost`, `input`, or `free`).

Note: Chat command start is only blocked *for starting* — players can still guess normally with `!g` regardless.

**Q: What does each Channel Points Bet Mode do?**

| Mode | Behaviour |
|------|-----------|
| `fixed` | Every redemption bets the exact **Fixed Bet Amount** set in the GUI |
| `cost` | Bet equals the reward's **channel point cost** |
| `input` | User types their bet into the reward text field (`rawInput`) |
| `free` | Bet is forced to 0 — free play, no points wagered |

**Q: A round is stuck as active after Streamer.bot restarted. How do I clear it?**

Use the mod command `!hmstop` (bound to the Stop Game action). The game state lives in memory, so a Streamer.bot restart already clears it — just start a new round normally.

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

No. A duplicate letter guess sends `MsgAlreadyGuessed` and is ignored — it does not cost lives or count toward anti-cheat.

---

## 💰 Points & Currency

**Q: What points system does the game use?**

The game auto-detects your currency in this priority order:
1. **StreamUP Currency Core** — reads `currency_ProductSettings` global.
2. **tawmae Loyalty Points** — reads `tawmae_Settings_LOYALTY POINTS` global.
3. **Config fallback** — uses `PointsVar` / `PointsName` from `configs.json`.

If you use a custom currency with a different variable name, set **Points Var** and **Points Name** in the Settings GUI → **Game** tab.

**Q: How are win points calculated?**

```
awarded = (RewardWinPoints + ChannelPointBet) × RoleMultiplier
```
`RoleMultiplier` starts at 1.0 and is adjusted by subscriber boost, VIP boost, and addicted penalty (see Role Management below).

**Q: What does "Reward Per Letter" do?**

If enabled, every correct letter guess awards **Points Per Letter** *in addition* to the base win reward. The letter reward is paid out immediately on each hit; the win reward is paid when the round ends.

---

## 👥 Role Management

**Q: How do Subscriber / VIP boosts work?**

Enable the boost in **Settings GUI → Game → Role Management** and set the percentage. The multiplier stacks additively:
```
multiplier = 1.0 + (SubBoost%) + (VipBoost%)  →  then clamped to ≥ 0
```
Example: 10% sub boost + 20% VIP boost on a 500-pt win = `500 × 1.30 = 650 pts`.

**Q: What is the "Addicted" system?**

Users on the addicted list face two penalties:
- **Points penalty** — their win multiplier is reduced by `AddictedPointsPenaltyPercent` (e.g. 50% → they earn half).
- **Guess cooldown** — they must wait `AddictedGuessIntervalSeconds` between guesses.

Manage the list with `!addicted add/remove <user>` (mods only) or edit **Addicted Users** directly in the GUI.

---

## 🧠 Hints

**Q: When does the game send a hint?**

A hint is sent to chat when **all** of these are true:
1. `HintEnabled` is on.
2. The word entry has a non-empty `Hint` field.
3. Wrong guesses have reached `HintAfterWrongGuesses`.
4. The percentage of revealed letters is below `HintStopRevealPercent`.

The hint fires once per wrong guess check (letter miss or word miss), not repeatedly.

**Q: How do I set hints per word?**

In the **Wordlist** tab of the Settings GUI, fill in the **Hint** column for each word. Words with an empty hint field will never trigger the hint message even if hints are enabled globally.

---

## 🛡️ Anti-Cheat & Cooldowns

**Q: What does the Anti-Cheat do?**

It limits how many guesses a single user can make within a rolling time window. If a user exceeds `AntiCheatMaxGuesses` guesses within `AntiCheatWindowSeconds`, they are blocked and sent the `MsgAntiCheat` message. Duplicate letter guesses do **not** count toward this limit.

**Q: What is the difference between Global Cooldown and User Cooldown?**

- **Global Cooldown** — applies to *all* users simultaneously after a guess.
- **User Cooldown** — applies only to the individual user who just guessed.

Both are set in the Settings GUI → **General** tab and are applied to the Streamer.bot command directly via `CPH.CommandSetGlobalCooldownDuration` / `CPH.CommandSetUserCooldownDuration`.

---

## 📋 Wordlist

**Q: How do I add or edit words?**

Open the Settings GUI → **Wordlist** tab. You can add, delete, search, and filter words directly in the table. Changes are held in memory until you click **Save** or **Save & Exit**.

**Q: What does Difficulty control?**

Each word has a `Difficulty` value (integer, minimum 1). The game only picks words where `MinDifficulty ≤ Difficulty ≤ MaxDifficulty`. Set both to the same value to pin a single difficulty tier.

**Q: What happens when all words in the difficulty range have been used?**

The used-word pool for the current difficulty range is automatically cleared and all words become eligible again. The game logs this as `"All N word(s) in current difficulty range exhausted — resetting used-word pool."` A fresh pick is then made immediately.

**Q: The wordlist file is missing or corrupt. Will the game break?**

No. On every round start the game checks whether `Hangman-Wordlist.json` exists and is valid. If it is missing or empty, a built-in default wordlist (24 words) is written to disk automatically.

---

## 🏆 Scoreboard

**Q: How is the leaderboard built?**

The leaderboard is updated whenever a user calls `!hmstats` (personal stats command). It is **not** updated automatically on win/loss — the player must check their stats at least once for their row to appear.

**Q: How do I exclude the broadcaster or bot from the scoreboard?**

In Settings GUI → **Other Tabs → Scoreboard**: uncheck **Include Broadcaster** and/or **Include Bot**. You can also add usernames to **Blocked From Board** (comma-separated) to exclude specific accounts (e.g. other bots).

**Q: How does the monthly auto-reset work?**

Enable **Reset All (Monthly)** or the per-platform toggles in the Scoreboard settings. On the first scoreboard command of each month (or on/after the configured `Reset Day`), all user stats for that platform are zeroed and the leaderboard is cleared. A chat message is sent and the reset date is saved to a persisted global so it only runs once per month.

---

## 🌐 Multi-Platform

**Q: Which platforms are supported?**

Twitch, YouTube, Kick, and Trovo. Enable the ones you stream on in Settings GUI → **General → Select Platform(s)**. The game determines the platform automatically from the incoming event.

**Q: Can the game run on multiple platforms at the same time?**

Yes. Each platform has its own independent `GameState` in memory, so a Twitch round and a YouTube round can be active simultaneously without interfering with each other. Points, stats, and leaderboards are also stored per-platform.

---

## 🔔 Update Notifications

**Q: How do update notifications work?**

On `Init()`, the script calls the GitHub releases API for the latest tag. If a newer version is found and you haven't been notified about it yet, a **Windows Toast notification** is shown. Clicking the toast opens the releases page. The last-notified tag is stored in a persisted global (`hm_LastNotifiedUpdateTag`) to prevent repeat toasts.

**Q: The toast notification fires on unrelated clicks. How is it filtered?**

The toast ID is set to `"Hangman Game"`. On `Execute()`, the script checks both the `toast.id` arg and compares it to that value before opening the browser. Any toast click not matching that ID is ignored or should get reported [here](https://github.com/aaskjer/Hangman-Game/issues).
