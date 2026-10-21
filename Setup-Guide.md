# Import
<p align="center"><img alt="grafik" src="https://github.com/user-attachments/assets/2c17f5a0-15f3-4396-a277-a66638017fb3"> </p> 

Click on `Import` in [streamer.bot](https://streamer.bot/) and drag the [HangmanGame.sb](https://github.com/aaskjer/Hangman-Game/releases) file into the window or copy paste the [import string](https://github.com/aaskjer/Hangman-Game/blob/main/Import-String.md).

<p align="center"><img alt="grafik" src="https://github.com/user-attachments/assets/4b853261-10bd-4f12-9f98-c3f99e6dc450"></p> 

Click on `Import` and `Yes` on both following popup messages.

## Enable Commands

<p align="center"><img alt="grafik" src="https://github.com/user-attachments/assets/a88a0e47-3c83-4806-8c67-75da34bf8b5b"></p> 

Enable all commands in the `Commands` tab of streamer.bot.

# Setup
<p align="center"><img alt="grafik" src="https://github.com/user-attachments/assets/8e6e6677-f7e8-45db-b9f8-6bfc88e06628"> </p> 
Right-Click on `Test` and click `Test Trigger` in the trigger section of `[HM] - Settings` to open the GUI of the game.

The window has one page per subject, picked from the rail on the left. Every field carries a tooltip, so hovering usually answers the question faster than this page does.

## Game
<p align="center"><img alt="Game page" src="https://github.com/aaskjer/Hangman-Game/blob/main/Assets/screenshots/game.png?raw=true"></p>

Lives, round duration and the time reminder, then when hints appear and what a win pays. Word scaling sits here too: harder and longer words earn a percentage on top, capped so a very long word cannot run away with your currency.

## Words
<p align="center"><img alt="Words page" src="https://github.com/aaskjer/Hangman-Game/blob/main/Assets/screenshots/words.png?raw=true"></p>

Add, edit and delete [words](https://github.com/aaskjer/Hangman-Game/blob/main/FAQ.md#-wordlist), each with a [hint](https://github.com/aaskjer/Hangman-Game/blob/main/FAQ.md#-hints), a category and a difficulty. Ctrl+click picks single rows, Shift+click a range, Del removes the selection. The difficulty range and the categories in play are folded away above the table.

## Overlay
<p align="center"><img alt="Overlay page" src="https://github.com/aaskjer/Hangman-Game/blob/main/Assets/screenshots/overlay.png?raw=true"></p>

A local HTML file that OBS loads as a browser source and that repaints itself as the round runs. *Setup Overlay in OBS* creates the scene and the source over your existing Streamer.bot OBS connection. The gallows, the colours, the scale and what is shown next to the word are all set here.

## Access
<p align="center"><img alt="Access page" src="https://github.com/aaskjer/Hangman-Game/blob/main/Assets/screenshots/access.png?raw=true"></p>

Who may start a round and who may not play at all, the start cooldowns, the guess rate limit, and the addicted list for regulars who should earn less and pace themselves. Channel Points live here as well.

Channel Points are a work in progress. The code came from another game and the author is not an affiliate, so it is not properly tested — if something misbehaves, please report it [here](https://github.com/aaskjer/Hangman-Game/issues).

## Messages
<p align="center"><img alt="Messages page" src="https://github.com/aaskjer/Hangman-Game/blob/main/Assets/screenshots/messages.png?raw=true"></p>

Every line the game posts, in one place. Pick a message from the list, edit it, and the preview underneath shows how it will read in chat. The chips below the box insert the placeholders that message accepts. The bot account and `/me` toggles at the bottom apply to the game's messages and to the cooldown replies separately.

## Board
<p align="center"><img alt="Board page" src="https://github.com/aaskjer/Hangman-Game/blob/main/Assets/screenshots/board.png?raw=true"></p>

How many players the scoreboard shows, who is left off it, and when it resets. Reset runs per platform or for all of them at once.

## Log
<p align="center"><img alt="Log page" src="https://github.com/aaskjer/Hangman-Game/blob/main/Assets/screenshots/log.png?raw=true"></p>

Every round, guess and payout, one file per day in the `Logs` folder. Filter by result or platform, or search the text.

---

[Frequently Asked Questions](https://github.com/aaskjer/Hangman-Game/blob/main/FAQ.md)
