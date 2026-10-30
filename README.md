<p align="center">
  <img src="https://github.com/aaskjer/Hangman-Game/blob/main/Assets/HM/Hangman-Game-Full2.png?raw=true" alt="Hangman Game" width="400" height="400">
</p>

<h1 align="center">Hangman Game</h1>

<p align="center">
  <b>Hangman for your chat.</b><br>
  One word, a row of blanks, and everyone guessing at once — on Twitch, YouTube and Kick.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/platforms-Twitch%20%7C%20YouTube%20%7C%20Kick-9146FF" alt="Platforms">
  <img src="https://img.shields.io/badge/tool-streamer.bot-0b73ff" alt="Streamer.bot">
  <img src="https://img.shields.io/github/license/aaskjer/Hangman-Game" alt="License">
  <img src="https://img.shields.io/github/v/release/aaskjer/Hangman-Game" alt="Release">
  <img src="https://img.shields.io/github/downloads/aaskjer/Hangman-Game/total" alt="Downloads">
</p>

<p align="center">
  <a href="https://github.com/aaskjer/Hangman-Game/releases"><b>Download</b></a> &nbsp;·&nbsp;
  <a href="Setup-Guide.md"><b>Setup Guide</b></a> &nbsp;·&nbsp;
  <a href="FAQ.md"><b>FAQ</b></a> &nbsp;·&nbsp;
  <a href="Import-String.md"><b>Import String</b></a>
</p>

---

## 📖 Overview

Hangman Game is a chat game for Streamer.bot. A round draws a word from the wordlist and shows it masked.
Chat guesses a letter or the whole word with `!g`: a wrong letter costs one life, a wrong word two. The
round ends when someone solves it, the lives run out or the clock does.

## ✨ Highlights

- ⚡ **Twists to switch on.** Blitz rounds bring less time, fewer lives and multiplied points. Evil Hangman
  swaps the word mid-round and keeps one revealed letter. Win streaks can pay a bonus.
- 📺 **An overlay that plays along.** The gallows fills in with every wrong guess and adapts to the number of
  lives. Evil Hangman's swap plays as a glitch, and chat only hears about it once the glitch is over. After
  an update the game refreshes the browser source in OBS by itself.
- 📚 **Wordlists from free sources.** The importer fills the wordlist with hints and difficulty: countries,
  capitals and rivers from Wikidata, German nouns from OdeNet, Pokémon from PokéAPI, video games from IGDB.
- 💰 **Payouts with a ceiling.** A base reward for the word, optional points per letter, and a bonus for harder
  and longer words, capped so a twenty-letter answer cannot drain your currency. Subscribers and VIPs can
  earn a boost.
- 🎫 **Channel Points on Twitch.** A redemption can start the round, with the stake taken from what the viewer
  typed or from the reward's cost. Wins pay out in your custom currency.
- 💬 **Control over chat.** Every line is editable with a preview. A rate limit catches guess spam, blocked
  users are ignored, and a per-platform scoreboard can reset monthly.

---

## Credits

- **Author:** [aaskjer on Twitch](https://twitch.tv/aaskjer) · [aaskjer.github.io](https://aaskjer.github.io/)
- **Built for:** [Streamer.bot](https://streamer.bot) by [nate1280](https://www.patreon.com/c/nate1280/home)
- **Works with:** [Currency Core by StreamUP](https://streamup.tips/product/currency-core) · [LOYALTY POINTS by tawmae](https://tawmae.xyz/loyalty-points)
- **Data & services:**
  - [Wikidata](https://www.wikidata.org) (CC0): countries, capitals, rivers and more
  - [OdeNet](https://github.com/hdaSprachtechnologie/odenet) (CC BY-SA 4.0): German nouns
  - [PokéAPI](https://pokeapi.co): Pokémon
  - [IGDB](https://www.igdb.com): video games
- **Community:** [Hangman Game on the Streamer.bot Discord](https://discord.com/channels/834650675224248362/1448251872744837203/1448251872744837203)
- **Thanks:** YOU ♡
