# v13.2.0 - 2026-11-08

### Added

- **Tells you about updates.** Once per Streamer.bot start, about 20 seconds in, one notice names new releases of
  this game, of the other UIFramework games and of UIFramework: in every chat whose broadcaster account is
  connected (Twitch, YouTube, Kick), or as a Windows notification while none is. The links are in Streamer.bot's
  log. *Tell me about updates* on the About page turns it off for this game. The game action is precompiled on
  application start, so the notice starts with Streamer.bot.
- The host block is UIFramework 1.4.0's; the settings window offers the newest UIFramework on its next open.

### Fixed

- **A timeout and the streaks a round broke are logged on the round's platform.** Those two lines took the platform
  of whoever ran the action last, so a Kick round that timed out could show up as Twitch on the Log page.

# v13.1.0 - 2026-11-07

### Added

- **The Log page shows every column.** Each line carries the guess, the word, the lives and the payout under the
  keys the Log page reads (Guess, Word, Wrong, Reward), and Round is the round's number. A win is logged once instead
  of twice, and the guess that ends a round and its payout carry the round too. Lines written before v13 read again
  as well (time, result, player, round) with UIFramework 1.0.0, and those from v13.0 fill the Guess, Word and Wrong
  columns.
- **A tour of the settings window.** The first time the window opens, a short tour rings one part after the other
  (the pages, saving, the customise mode) and says what it does; the **?** at the top right shows it again.
- **UIFramework 1.0.0.** Problems the window finds sit behind a red **i** in its header instead of on the About or
  Log page, and *How to play*, *Log* and *About* sit together at the foot of the page list. The settings action
  offers the new framework on its next open.

### Changed
- **Messages from an old `messages.json` show in the settings window.** Before, only the game read that file: the
  window showed the default texts in their place, and a save wrote those over your messages. The messages now move
  into `configs.json` on the first run of the game or of the window, and the file stays as `messages.json.migrated`.
- **Import words shows only what the source uses.** *Topic* appears for Wikidata and OdeNet, the Q-ID for
  Wikidata's custom topic, the generations for PokéAPI and the IGDB fields for IGDB; before, the others stood there
  greyed out.
- **The settings action is UIFramework's stub 0.2.0.** When the game action hangs, the settings window waits 30
  seconds for its answer, then carries on and can be closed; before, it waited for good. A game that has not
  described itself for a minute no longer locks the settings until Streamer.bot restarts: the next run opens the
  window.
- The host block is UIFramework's current one (1.0.0). It names every type in full, so it no longer depends on the
  action's `using` lines, and it can declare a tour of the window. Nothing changes in the window.

# v13.0.0 - 2026-11-07

### Added
- **Try the demo** and **Show a sample round** on the Overlay page, next to the OBS buttons. *Try the demo* opens
  the overlay in your browser, playing made-up rounds without Streamer.bot in the colours and sizes you saved; in OBS,
  `&demo=1` at the end of the source's address does the same, to place and size it. *Show a sample round* plays one
  made-up round on the live overlay, the one in OBS included, letters, wrong guesses, an Evil Hangman swap and a win,
  then puts the overlay back as it was; it refuses while a real round runs. The overlay is version 10 and replaces an
  older copy on the next round, keeping that one as a `.bak`.

### Changed
- **The settings window is UIFramework's.** `[HM] - Settings` is now UIFramework's generic settings action: it loads
  `UIFramework.dll` from the [UIFramework releases](https://github.com/aaskjer/UIFramework-Streamer.bot/releases) on
  the first open (it asks first, checks the file's hash) and keeps it current, and the game itself declares every
  setting, message and page of the window in one table (`UiSchema.Table` in `Hangman-Game.cs`, carried by the
  scoreboard action too). The pages are the same - Game, Words, Overlay, Access, Messages, Board, Log - plus a help
  page and a customise mode (the pencil: rename, move or hide pages and fields, accent colour, language).
- **English or German.** With the window's language set to `de`, every setting, section, page and message reads in
  German, the word list's column headers and the name-list placeholder included. The help and about pages stay
  English.
- `configs.json` keeps working as it is: the window reads the old names (`MaxWrongGuesses`, `MsgGameStart`, …)
  through aliases and writes the new ones on the first save. `OverlayBackgroundOpacity` is stored as a percent now
  (an old 0.5 reads as 50). `DarkMode` moves into `uiframework.layout.json`; the Egg mode is gone (the framework's
  customise mode has a rainbow of its own). `messages.json` from before v12 is still read for a message the settings
  file does not hold.
- **The log in the framework's format.** Lines now read `[time] RESULT | User: … | Platform: … | Round: … | …`, which
  the Log page filters by result, platform and day; files from before v13 show as raw lines. A *Write a log file per
  day* switch sits on the Log page.
- **Importing words** is a section of the Words page: source, topic, language, limits and difficulty as fields, then
  *Import now*. The fetched words skip what the list already holds, land in `Hangman-Wordlist.json` and show in
  the list right away. Gone with the old window: the preview with ticks before anything is added.
- **The word list is a table,** edited in place: a click into a cell edits it, a search box above, several rows
  picked and removed at once (Remove row or Delete). The IGDB client ID and secret are typed into the import fields; they are still
  kept in Streamer.bot's global variables after the first import, never in `configs.json`.
- *Categories in play* became *Categories left out*: type a category name and press Enter to leave it out; the
  checkbox list of the old window needed the word list at hand.
- What the old window did on its own - the unsaved-changes counter, the question before changes are discarded,
  number fields that refuse text, the theme - is UIFramework's now and behaves as in the other chat games. The
  ranges the old window enforced on save (at least one life, chances within 0..100, the overlay scale not below
  10 %, 1 to 25 places on the board, …) are applied by the game when it reads the file, so a hand-edited value
  cannot push it out of range either.
- The OBS buttons are *Create the browser source in OBS* and *Refresh the overlay in OBS*; both answer in the
  window's footer instead of a message box. *Open the word list folder* and *Open the overlay folder* buttons.
- The game and the scoreboard carry UIFramework's current host block (0.9.11). Needs UIFramework 0.9.8 or newer for
  the word table; the settings action fetches it.

### Added
- **Free mode.** A switch on the Game page (and a pill in the header): letters and wins pay nothing while it is on.
  `!SetFreeHangman` and `!EndFreeHangman` switch it from chat (moderators; two new commands in the import, disabled
  until you enable them), with a Started and an Ended message and the custom triggers `Hangman: Free Game Start` and
  `Hangman: Free Game End`. An action of your own can do the same with an Execute C# Method on `StartFreeGame` or
  `EndFreeGame`.
- **The round as arguments.** `Hangman: Game Win`, `Game Loss`, `Game Stopped` and `Timeout` now carry the round:
  `roundOutcome`, `roundWord`, `roundCategory`, `roundDifficulty`, `roundLetters`, `roundPlatform`, `roundWinner`,
  `roundWinnerId`, `roundPoints`, `roundWrongGuesses`, `roundMaxWrong`, `roundBlitz`, `roundFree`, `roundCurrency`,
  `roundParticipants`; the same in the globals `hm_last…`, and `GetLastRoundResult` (an Execute C# Method on the
  game action) hands them to any other action.

---

# v12.1.0

### Changed
- **A new colour picker** for the overlay colours. A click on the swatch next to a colour, or on the 🎨 button beside it, opens the picker right under the field instead of a separate window with a colour wheel: a field for saturation and brightness and a hue bar, both to click or drag, sliders for red, green and blue (the mouse wheel nudges them by one), and a row of ready colours. Every change goes straight into the hex box, and typing a hex code moves the picker along. Done or a click elsewhere closes it.

---

# v12.0.1

### Fixed
- A `configs.json` that the game script happens to be holding open is waited out instead of failing on the first try, both when reading and when writing.
- A damaged `configs.json` could overwrite the good `configs.json.bak` on the next save. The file being replaced only becomes the backup when it parses itself, so the last readable copy is never lost to a broken one.
- Three places still read the settings through the path that throws on a damaged file: the first time anything in the script is touched, Reset to defaults, and closing the window. All three now fall back to the backup or the defaults, the way the window already did.
- The global and per-user cooldowns are handed to Streamer.bot as one pair rather than two separate numbers, and once more after the window has closed, so a save made in the last moment is not lost.
- A settings window that failed while it was being built left its thread running with nothing to show. The dispatcher is now shut down in either case.

---

# v12.0.0

### Added
- **Word import.** An Import button on the Words page fetches words with hints from four free sources, shows them for review and adds the ticked ones to a category of your choice:
  - **Wikidata** (CC0): countries, capitals, big cities, rivers, mountains, chemical elements, musical instruments, sports, dog breeds, or any Wikidata class by its Q-ID. German or English, best known first.
  - **OdeNet**, the open German WordNet (CC BY-SA 4.0): animals, plants, food, objects, people & jobs, body, places, materials, nature and feelings, each with its German definition as the hint. Downloaded once and cached for a month.
  - **PokéAPI**: Pokémon names in German or English with their category as the hint, by generation.
  - **IGDB**: video game titles, best rated first. Needs the Client ID and Secret of a free Twitch developer app; both are kept in Streamer.bot's global variables, not in `configs.json`.
  - Words with digits or symbols, too many letters or too many words are skipped, accents are folded, umlauts can be written out, and a hint that names its own answer is replaced or masked. Difficulty is rated automatically from length, rare letters and how well known the word is, or set by hand.
- **Blitz rounds.** Shorter time, fewer lives, and every point pays a multiplier. A round turns into a blitz by chance, or a mod calls one with `!hm blitz`. The overlay shows a pulsing BLITZ badge.
- **Evil Hangman.** Once chat has lost a set share of its lives, the overlay glitches and the word is swapped. Every guess is wiped except one revealed letter, which the new word always contains. Lives either stay lost or refill, as you choose. At most once per round.
  - The glitch runs in stages slow enough to read on a browser source at a low frame rate: the old word breaks up, everything scrambles under scanlines with a blinking SIGNAL LOST, the new word decodes, and the surviving letter lights up in the accent colour. Its length is a setting (3 seconds by default, 0 for no effect).
  - Chat is told about the swap only once the glitch has played out. Until then guesses and `!hmr` are ignored and the "already running" reply stays quiet, so nothing in chat gives the new word away early. If the round ends meanwhile, the message is dropped.
- **Streaks.** Each solved word adds to the solver's streak; it breaks when a round they guessed in is lost or times out. Winning streaks are announced, can pay a capped bonus, and show up as an extra line in the personal stats (`!hmp`). `{streak}` and `{beststreak}` also work in the board and stats messages, and the monthly reset clears them.
- Seven new chat messages on the Messages page, under Blitz, Evil Hangman and Streaks.
- **Refresh Overlay in OBS** button on the Overlay page. It reloads every OBS browser source that shows `hangman_overlay.html`.

### Changed
- The overlay is now version 9. It replaces the old file on its own and keeps it as `hangman_overlay.html.v5.bak`.
- OBS keeps running the overlay page it loaded, even after the file on disk is replaced, so an update used to stay invisible until the browser source was refreshed by hand. The game now refreshes those browser sources itself over Streamer.bot's OBS connection after an update and once per Streamer.bot session, and the page reloads itself whenever a newer version is on disk.
- A round ended with `!killhm` shows **GAME OVER - MODERATOR STOPPED** in orange on the overlay, instead of the red "GAME OVER - <word>" of a lost round.

---

# v11.0.0

### Changed
- Settings window rebuilt: icon rail instead of the sidebar, status pills in the header, sliding switches, `Ctrl+S` to save and `Ctrl+1`…`8` to switch pages.
- 15 pages reduced to 8: Game, Words, Overlay, Access, Messages, Board, Log, About.
- All 40 chat messages moved to one Messages page, with a live chat preview and placeholder chips.
- Bot-account and `/me` toggles moved to Messages, separately for game and cooldown replies.
- Update check runs in the background; a pill in the header replaces the popup that used to block the window.
- Wordlist and action-log tables follow the window theme instead of showing up white.
- Unsaved changes show as a status line rather than a count.

### Added
- A damaged `configs.json` no longer blocks the window: it is kept as `configs.json.broken` and the settings come from `configs.json.bak`, which every save now leaves behind, or from the defaults.
- Only one settings window opens; a second trigger brings the existing one to the front.
- The colour wheel takes a click or a drag to pick hue and saturation.

### Removed
- Platform selection. Twitch, YouTube and Kick all play — restrict the game per platform in Streamer.bot instead.
- Sidebar search, Reset Page, and the placeholder chips under every message box.

### Fixed
- Upgrades from v9 forced a fixed channel point stake: the bet-mode migration ran only in the settings window, not in the game action.
- 25 Windows references in the export were relative to the exporting Streamer.bot's folder and were dropped on import elsewhere.
- A running countdown kept going after the action was destroyed; it is cancelled now.
- Eight defaults disagreed between the game action and the settings window, among them round duration, maximum difficulty and the subscriber and VIP boosts.
- `MsgGameStart` opened with a broken character instead of 🎮.
- The update check compared tags for inequality and would offer a downgrade; it compares versions now.

---

# v10.0.0

## ✨ New Settings GUI

The settings window has been rebuilt from the ground up.

- **Sidebar navigation** replaces the tab strip. Everything is grouped under **Setup**, **Game**, **Players**, **Results** and **Help**, and nothing is hidden inside a sub-tab anymore.
- **Search.** The box at the top of the sidebar filters by page name, setting label *or* setting key — type `cooldown`, `payout` or `AntiCheatWindowSeconds` and it takes you straight there.
- **Messages live next to the feature that sends them.** The hint text is on the Hints page, the anti-spam warning on Anti-Spam, the cooldown replies on Start Cooldown. Tuning one feature no longer means hopping between tabs.
- **Placeholder chips.** Every message box lists its available placeholders underneath. Click `{user}` or `{masked}` to drop it in at the cursor instead of copying it out of a tooltip.
- **Unsaved changes are visible.** The footer tells you how many settings you have changed, and Cancel now asks before throwing them away.
- **Reset Page** restores just the page you are looking at. The old all-or-nothing reset is still there, but you no longer have to wipe your OBS paths and Reward ID to undo one bad number.
- **Warnings on save** for settings that quietly cancel each other out — a hint threshold higher than your max wrong guesses (the round always ends before the hint can fire), a difficulty range that matches no words, Channel Point start switched on while the Reward ID is still the placeholder, and so on.
- **Wordlist page**: multi-select with Ctrl+click / Shift+click / Ctrl+A and Delete-key support, a live count of how many words your filter is showing, and the difficulty range and category filters moved directly above the words they apply to.
- **Folder buttons** on the Wordlist and Action Log pages, so you can jump to the files on disk.

## 📺 OBS Overlay — new

Hangman can now draw itself on stream. A local HTML file acts as an OBS Browser Source and repaints itself as the round plays out — no refreshing, no scene switching, nothing to click mid-game.

**Get it running in one click.** The Overlay page has a *Setup Overlay in OBS* button that creates the scene and the browser source for you over your existing Streamer.bot OBS connection, pointed at the right file. Prefer to do it by hand? The folder is one button away and the file is a plain local `.html`.

**The gallows draws itself.** The figure is vector art that fills in as chat gets it wrong, and it adapts to however many lives you allow:

- **Proportional** — the classic six body parts, spread evenly across your max wrong guesses, so the last life always completes the figure
- **Classic + Gallows** — ten steps, adding the base, post, beam and rope as separate stages, which suits higher life counts
- **Auto** — picks between the two based on your settings

**Show exactly what you want.** Category, lives as hearts, guessed letters, wrong letters, wrong words and the hint are each their own toggle, so the overlay can be a full scoreboard or just a word and a gallows.

**Make it yours.** Independent colours for the gallows, the figure, the text and the accent, plus a background colour with its own opacity — transparent out of the box, so it drops onto any scene. Scale the whole overlay and the text separately to fit the space you have.

**Long words fit.** The masked word scales down to the width of your browser source and wraps if it has to, so a twenty-letter answer stays on screen instead of running off the edge. It is always drawn in uppercase for readability at a distance.

**It follows your rules.** The hint only appears once the game actually reveals it in chat, honouring *Use Hints*, *Hint After Wrong Guesses* and *Hints Stop Reveal* — so the overlay never spoils an answer your chat has not earned yet.

**It knows when to leave.** When a round ends the overlay announces `SOLVED`, `GAME OVER` or `TIME UP` with the answer, then fades out after a delay you choose — or set it to `0` and it disappears the moment the round is done.

## 🎯 Channel Points — the stake is now detected automatically

**The Bet Mode dropdown is gone.** The stake is read from the redemption itself:

1. The viewer typed a number into the reward → that number is the stake
2. Otherwise, the reward costs Channel Points → the reward cost is the stake
3. Otherwise → no stake, the round is free to play

The old Fixed mode lives on as a single field, **Fixed Stake Override**: leave it at `0` to let detection do its thing, or set anything above `0` to force every redemption to stake exactly that much.

Whichever route was taken is written to the action log on every redemption (`Stake mode: input`), so you can confirm what actually happened.

**Existing setups are migrated automatically.** If you were on Cost, Input or Free, the override is set to `0` and detection takes over. If you were on Fixed, your amount is kept exactly as it was.

## 📋 Action log

- **Logs are kept forever.** One file per day is still written to the `Logs` folder, and nothing is deleted automatically anymore.
- The **Log Retention (Days)** setting has been removed.

## 🔠 Wordlist

- **The two default wordlists now match.** A fresh install and the *Load Defaults* button used to hand out completely different sets of words — 24 generic ones on install, a different 15 from the button. Both now ship the same list.

## ⚠️ Upgrade notes

- Removed settings: **Log Retention (Days)** and **Bet Mode**.
- **Fixed Bet Amount** is now **Fixed Stake Override** and defaults to `0` (automatic). See the Channel Points section above for how existing values are carried over.
- Your `configs.json` carries over as-is. Settings that no longer exist are ignored on load and dropped the next time you save.
