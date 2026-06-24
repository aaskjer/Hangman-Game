<p align="center"><img src="https://github.com/aaskjer/Hangman-Game/blob/main/Assets/HM/Hangman-Game-Full.png?raw=true" alt="HM banner" width="400" height="400"></p>

Hangman is a chat game built for streamer.bot, supporting twitch, kick, trovo and youtube.


---

# Overview

- Works on Twitch, YouTube, Trovo, and Kick. 
- Start a Hangman round and let chat guess with letters or full words using `!g <letter or word>`. 
- Supports custom word categories, hints, and difficulty ranges through `Hangman-Wordlist.json`. 
- Adjustable round rules including max wrong guesses, round duration, minimum difficulty, and maximum difficulty. 
- Rewards viewers with points for solving the word, and optionally for revealing correct letters. 
- Built-in anti-cheat protection with rapid-guess limits and duplicate-guess handling. 
- Exclude specific Streamer.bot groups or individual users from playing. 
- Optional subscriber and VIP reward boosts. 
- Optional “addicted” handling with slower guess intervals and reduced rewards. 
- Hint system with configurable trigger point and reveal limit. 
- Time reminder support during active rounds.
- Auto-detects [Currency Core by StreamUP](https://streamup.tips/product/currency-core) and [LOYALTY POINTS by tawmae](https://tawmae.xyz/loyalty-points).   
- Every chat message is editable directly in the settings GUI, including start, win, loss, hint, anti-cheat, and timeout messages. 
- Persistent log file with every Hangman result, filterable and searchable in the built-in log viewer. 
- Built-in WPF settings window with dark/light mode, tab navigation and integrated wordlist editor. 

---

## 🎮 How It Works

Start a round of Hangman, and chat guesses by using `!g <letter>` or `!g <word>`. The game tracks lives, wrong guesses, hints, and round timeout until the word is solved, the round times out, or all lives are lost. 

| Event | Trigger | Result |
|---------|---------|--------|
| 🎮 **Start** | A mod starts a round | A word is picked from the configured wordlist and shown as a masked word.  |
| ✅ **Correct Letter** | A viewer guesses a valid letter in the word | The letter is revealed, and optional letter rewards can be given.  |
| 🎉 **Correct Word** | A viewer guesses the full word | The round ends and the winner gets the configured reward.  |
| ❌ **Wrong Guess** | A viewer guesses a wrong letter or word | Lives are reduced depending on the guess type.  |
| 💡 **Hint** | Hint conditions are met | The configured hint is shown to chat.  |
| 💀 **Game Over** | Lives reach zero | The word is revealed and the round ends.  |
| ⏰ **Timeout** | Round duration expires | The word is revealed and the round ends.  |

---

## 🎫 Channel Points Mode (Twitch only)

When `Use Channel Points To Start` is enabled, the start command is effectively disabled — only viewers who redeem the configured Channel Point reward can start a game, but everyone can play. Four modes are available:

| Option | Info |
|---------|---------|
| **Cost** | The reward's point cost is the bet |
| **Input** | The viewer types their bet into the reward's text field |
| **Fixed** | Every redemption bets a fixed configured amount |
| **Free** | All redemptions play for free regardless of cost |

---

## Get Started

[Setup Guide](https://github.com/aaskjer/Hangman-Game/blob/main/Setup-Guide.md)

[Frequently Asked Questions](https://github.com/aaskjer/Hangman-Game/blob/main/FAQ.md)

[Import String for streamer.bot](https://github.com/aaskjer/Hangman-Game/blob/main/Import-String.md)

[Download](https://github.com/aaskjer/Hangman-Game/releases)

---

## Credits & Links


[aaskjer on Twitch](https://twitch.tv/aaskjer)

[Hangman Game on SB Discord](https://discord.com/channels/834650675224248362/1448251872744837203/1448251872744837203)

[Streamer.bot](https://streamer.bot) / [nate1280](https://www.patreon.com/c/nate1280/home)

YOU ♡
