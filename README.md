<p align="center"><img src="https://github.com/aaskjer/Hangman-Game/blob/main/Assets/HM/Hangman-Game-Full2.png?raw=true" alt="HM banner" width="400" height="400"></p>

<p align="center">
  <b>Hangman for your chat.</b><br>
  One word, a row of blanks, and everyone guessing at once — on Twitch, YouTube and Kick.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/https%3A%2F%2Fimg.shields.io%2Fbadge%2Fany_text-Twitch%2C_Kick%2C_YouTube-white?label=supported%20plattform" alt="Platforms">
  <img src="https://img.shields.io/badge/https%3A%2F%2Fimg.shields.io%2Fbadge%2Fany_text-streamer.bot-white?label=tool&color=%230b73ff" alt="streamer.bot">
  <img src="https://img.shields.io/github/license/aaskjer/Hangman-Game" alt="License">
  <img src="https://img.shields.io/github/v/release/aaskjer/Hangman-Game" alt="Release">
  <img src="https://img.shields.io/github/downloads/aaskjer/Hangman-Game/total" alt="Downloads">
</p>

---

## 🎮 How It Works

A round starts, a word is drawn, and chat guesses with `!g <letter>` or `!g <word>`. Lives, wrong guesses, hints and the round clock tick along until somebody solves it or the round runs out.

| Event | Trigger | Result |
|---------|---------|--------|
| 🎮 **Start** | A round is started | A word is picked from the wordlist and shown masked. |
| ✅ **Correct Letter** | A viewer names a letter in the word | The letter is revealed, and a letter reward can be paid. |
| 🎉 **Correct Word** | A viewer names the whole word | The round is won and the payout goes out. |
| ❌ **Wrong Guess** | A viewer misses | Lives drop — a wrong letter costs one, a wrong full word two. |
| 💡 **Hint** | Enough wrong guesses, not too much revealed | The word's hint goes to chat. |
| 💀 **Game Over** | The last life is gone | The word is revealed and the round ends. |
| ⏰ **Timeout** | The round duration runs out | The word is revealed and the round ends. |
| 🛑 **Stopped** | A moderator types `!killhm` | The word is revealed and the round ends. |
| ⚡ **Blitz** | By chance, or a moderator types `!hm blitz` | A short round: less time, fewer lives, every point multiplied. |
| 👾 **Glitch** | Evil Hangman — chat has burned through part of its lives | The overlay glitches and the word is swapped. One revealed letter survives. |
| 🔥 **Streak** | A player solves again before losing a round | The streak grows, can pay a bonus, and shows in `!hmp`. |

Out of the box only moderators and the broadcaster can start a round. Turn off *Require Mod To Start* to let everyone. Blitz, Evil Hangman and streaks are optional and stay off until you switch them on.

---

## 📺 On Stream

A local HTML file that OBS loads as a browser source and that repaints itself as the round plays out. The gallows fills in with every wrong guess and adapts to however many lives you allow. Category, lives, guessed letters, wrong letters, wrong words and the hint are each their own toggle, so the overlay can be a full scoreboard or just a word and a figure.

Colours, scale and background opacity are yours; it starts out transparent, so it drops onto any scene. One button creates the scene and the browser source in OBS for you.

A blitz round wears a pulsing badge. Evil Hangman's word swap plays as a glitch: the old word breaks up, the panel scrambles under scanlines, the new word decodes and the surviving letter lights up — and chat only hears about it once the glitch is over, so nobody gets spoiled. A round a moderator ends shows *GAME OVER - MODERATOR STOPPED*.

After an update the game refreshes the browser source in OBS by itself, so the new overlay shows up without you touching OBS.

---

## ⚙️ What You Can Change

**The round** — lives, duration, a reminder before the clock runs out, and the difficulty range words are drawn from.

**The payout** — a base reward for solving the word, optionally points per revealed letter. Harder and longer words earn a percentage on top, capped so a twenty-letter answer cannot run away with your currency. Subscribers and VIPs can earn a boost.

**The words** — `Hangman-Wordlist.json`, edited in the window: word, hint, category and difficulty, with multi-select and filters. An importer fills it from free sources, hints and difficulty included: countries, capitals, rivers and more from Wikidata, German nouns with their meaning from OdeNet, Pokémon from PokéAPI, video games from IGDB.

**The twists** — blitz rounds with a payout multiplier, Evil Hangman's mid-round word swap, and win streaks with an optional bonus.

**The chat** — every line the game posts, in one place, with a preview of how it will read and chips for the placeholders each message accepts.

**The regulars** — the addicted list earns less and waits longer between guesses. A rate limit catches guess spam. Blocked users and Streamer.bot groups are ignored entirely.

**The record** — one log file per day with every round, guess and payout, searchable in the built-in viewer. A per-platform scoreboard with personal stats and an optional monthly reset.

Currency Core by [StreamUP](https://streamup.tips/product/currency-core) and [LOYALTY POINTS by tawmae](https://tawmae.xyz/loyalty-points) are picked up automatically; any other currency takes one variable name.

---

## 🎫 Channel Points (Twitch only)

With `Use Channel Points To Start` on, the chat command no longer starts a round — a redemption does. Everyone still guesses normally.

The stake comes from the redemption itself:

| Where it comes from | When |
|---------|---------|
| **What the viewer typed** | The reward asks for text and the viewer entered a number |
| **The reward's cost** | No input, but the reward costs channel points |
| **No stake** | Neither applies — the round is free to play |

*Fixed Stake Override* sits above all three: leave it at `0` for the detection above, or set any value to make every redemption stake exactly that.

Streamer.bot cannot pay channel points back out, so a win is paid in your custom currency, with the stake converted at a multiplier you set.

---

## 🚀 Get Started

| | |
|---|---|
| 📥 | [**Download**](https://github.com/aaskjer/Hangman-Game/releases) — the `.sb` file for Streamer.bot |
| 📋 | [**Import String**](https://github.com/aaskjer/Hangman-Game/blob/main/Import-String.md) — paste it instead, if you prefer |
| 🧭 | [**Setup Guide**](https://github.com/aaskjer/Hangman-Game/blob/main/Setup-Guide.md) — import, enable the commands, open the window |
| ❓ | [**FAQ**](https://github.com/aaskjer/Hangman-Game/blob/main/FAQ.md) — commands, payouts, hints, troubleshooting |

---

## 💜 Credits & Links

[aaskjer on Twitch](https://twitch.tv/aaskjer)

[aaskjer.github.io](https://aaskjer.github.io/)

[Hangman Game on SB Discord](https://discord.com/channels/834650675224248362/1448251872744837203/1448251872744837203)

[Streamer.bot](https://streamer.bot) / [nate1280](https://www.patreon.com/c/nate1280/home)

Word import: [Wikidata](https://www.wikidata.org) (CC0) · [OdeNet](https://github.com/hdaSprachtechnologie/odenet) (CC BY-SA 4.0) · [PokéAPI](https://pokeapi.co) · [IGDB](https://www.igdb.com)

YOU ♡
