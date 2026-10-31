![Stop Chop and Roll](logo.svg)

# Stop Chop and Roll

A Slice & Dice-style dice-hero duel for two friends, with a 5-wave single player mode for practice. Draft a party of heroes (each hero is one six-sided die), roll, reroll, and chop your rival's life to zero.

**Play:** https://miraclewhip3.github.io/stop-chop-roll/ (public copy). Developed in the private Cheezy Games repo and copied here.

## Vision
*Pulled together from the 2026/10/30 design rounds (as "Dice Duel"). Not yet confirmed as a written statement.*

- **Fantasy:** you lead a small party of dice heroes in a quick, open duel against a friend.
- **Feeling:** clever reads, tense swings, laughing at a bad roll. A match is about 15 minutes.
- **Pillars**
  1. **Readable at a glance.** Dice show an icon and a big number; details live in hover tips.
  2. **Open information, real choices.** No blind guessing: one player commits in the open, the other responds. Every die is a choice between *race* (hit life) and *control* (kill a hero).
  3. **Short matches with friends.** 5 rounds, rising life, rewards in between.
  4. **Zero setup.** Room codes, no accounts, no server.
- **Non-goals:** hidden-information mind games, long campaigns, deck-building complexity, accounts or servers.
- **When in doubt:** pick the option that keeps a turn readable in a few seconds.

## Contents
- [How to play](#how-to-play)
- [Modes](#modes)
- [Faces](#faces)
- [Heroes](#heroes)
- [Spells](#spells)
- [Rewards: level-ups, items, relics](#rewards-level-ups-items-relics)
- [Theme cards](#theme-cards)
- [Multiplayer](#multiplayer)
- [Saving and audio](#saving-and-audio)
- [Dev tools](#dev-tools)
- [Design decisions](#design-decisions)
- [Assumptions made in this prototype](#assumptions-made-in-this-prototype)
- [To review later](#to-review-later)
- [Roadmap to alpha](#roadmap-to-alpha)

## How to play
1. **Draft** 4 heroes.
2. Each round both players have a **life total**: 12 in round 1, then 14, 16, 18 and 20.
3. **Turns alternate.** The first player rolls all their dice, clicks dice to **keep** them, and **rerolls** the rest (2 rerolls). Then every die gets a **target** and the player **commits**, in the open.
4. The second player rolls and responds, seeing exactly what's coming.
5. All committed dice **resolve together**: shields first, then damage, then heals. A hero killed this turn still uses its die. Next turn the other player goes first.
6. A player loses the round at **0 life**, or when **all their heroes are down**. Heroes come back at full HP next round; mana resets.

**Controls:** click a die to keep it (roll step). After rolling, every die has a default target (damage → enemy life, shield → your life, heal → the ally who needs it most). Click a die, then a highlighted hero card or life box, to change it. Click a ✦ spell button to cast. Hover anything for details. `Esc` cancels a selection.

## Modes
- **Single player (5 waves):** pick 4 of 8 random heroes, then fight 5 AI rival parties in a row. Each rival party is new, gets 4 random heroes, and has as many upgrades as you've had (wave 3 rivals have 2 random level-ups or items). After each win you pick 1 of 5 rewards. A loss ends the run, but you can **retry the wave** from its start. The rival rolls first each wave. The run autosaves.
- **Duel a friend:** snake draft from 10 heroes (A B B A A B B A), best of 5 rounds, first to 3. After each round the loser picks a reward first, then the winner picks from the 4 left. The loser also chooses who goes first next round. Round 1 starts with the player who picked second in the draft.

## Faces
| Face | Effect |
|---|---|
| ⚔ Damage N | Deal N damage to one enemy hero or the enemy player's life. |
| 🛡 Shield N | Block N damage to one of your heroes or your life, this turn. |
| ✚ Heal N | Restore N HP to one living ally hero, after damage. |
| ✦ Mana N | Add N to your party's mana pool for the round (added as soon as you finish rolling). |
| ✕ Blank | Does nothing. A level-up turns it into ⚔1. |

## Heroes
| Hero | HP | Faces | Spell |
|---|---|---|---|
| 🗡️ Knight | 8 | ⚔3 ⚔3 ⚔2 🛡2 🛡2 ✕ | — |
| 🏹 Archer | 5 | ⚔3 ⚔2 ⚔2 ⚔2 ⚔1 ✕ | — |
| 🛡️ Paladin | 7 | 🛡3 🛡2 ✚2 ⚔2 ⚔1 ✦1 | Sanctuary |
| 👹 Ogre | 12 | ⚔4 ⚔4 ⚔3 ✕ ✕ ✕ | — |
| 🕯️ Cleric (caster) | 6 | ✚3 ✚2 ✚2 🛡1 ✦1 ✦1 | Mass Mend |
| 🥷 Rogue | 5 | ⚔3 ⚔3 ⚔2 ⚔1 ✦1 ✕ | Backstab |
| 🧙 Wizard (caster) | 4 | ✦2 ✦2 ⚔4 ⚔3 ✦1 ✕ | Firestorm |
| 🎻 Bard (caster) | 5 | ✦2 ✦1 ✚1 🛡1 ⚔1 ✕ | Anthem |
| 🥋 Monk | 6 | ⚔2 ⚔2 🛡2 ✚2 ✚1 ✕ | — |
| 💀 Necromancer (caster) | 5 | ⚔2 ⚔2 ✦2 ✦1 ✚1 ✕ | Soul Harvest |
| 🌿 Druid (caster) | 6 | ✚2 🛡2 ✦2 ✦1 ⚔2 ✕ | Barkskin |
| 🪓 Berserker | 6 | ⚔5 ⚔3 ⚔3 ⚔2 ⚔1 ✕ | — |

Casters always have a spell; the Paladin and Rogue are the non-casters that have one.

## Spells
A spell is cast in addition to the hero's die, once per turn, by a living hero. Mana is spent when you click the spell (click again to cancel before committing).

| Spell | Cost | Effect |
|---|---|---|
| Sanctuary | 4 | Shield 2 on every ally hero. |
| Mass Mend | 3 | Heal 2 on every ally hero. |
| Firestorm | 4 | 2 damage to every enemy hero. |
| Backstab | 2 | 3 damage to the enemy hero with the lowest HP (checked at resolve). |
| Anthem | 3 | +1 to every other die you committed this turn. |
| Soul Harvest | 3 | 3 damage to the enemy player. |
| Barkskin | 3 | Shield 4 on your life. |
| Spark | 2 | 2 damage to the enemy player (from the Spark Scroll item). |

## Rewards: level-ups, items, relics
Each reward row has 5 options: 2 level-ups, items, and sometimes (35%) one relic nobody owns yet.

- **Level up (generic):** +2 max HP and upgrade one face on any hero. A blank becomes ⚔1 (⚔2 under Sharp Edges); any other face gets +1.
- **Items** fit any hero, one per hero. Equipping over an item sends the old one to your **bag**. Between rounds, click an item and then another slot to move it.

| Item | Effect |
|---|---|
| 🔪 Whetstone | ⚔ faces +1 |
| 🔰 Buckler | Blank faces become 🛡2 |
| 🦇 Vampire Fang | When this hero deals damage, it heals 1 after damage |
| ⛓️ Plate Armor | +3 max HP |
| 💎 Mana Crystal | Blank faces become ✦1 |
| 🌱 Healing Herb | ✚ faces +1 |
| 🧱 Tower Shield | 🛡 faces +1 |
| 📜 Spark Scroll | Hero can cast Spark |

| Relic (party) | Effect |
|---|---|
| 🎲 Lucky Dice | +1 reroll every turn |
| 💗 Heart Locket | +3 life every round |
| ⛲ Mana Well | Start every round with 2 ✦ |

## Theme cards
One theme is flipped per match and shown on the draft screen and in the top bar.

| Theme | Rule |
|---|---|
| Glass Bones | Every hero −2 max HP; ⚔ faces +1 |
| Thick Skin | Every hero +3 max HP |
| Arcane Surge | Spells cost 1 less (minimum 1) |
| Lucky Day | 3 rerolls per turn |
| Short Fuse | Player life is 4 lower every round |
| Sharp Edges | Blank faces deal ⚔1 |

## Multiplayer
Same approach as the Multiplayer Test from Cheezy Games:
- **WebRTC peer-to-peer via PeerJS 1.5.4** (unpkg). The free PeerJS broker only introduces the browsers. Room ids are `cheezy-scr-<CODE>`; codes are 4 letters without I or O.
- **The host is authoritative.** The host runs the rules (`act(state, player, action)`), and the guest only sends actions and draws the state it receives. The RNG seed is stripped before the state is sent.
- Single player uses the same engine, with the AI sending actions for seat 2.
- 2 players per room. If either player leaves, the duel ends. Either player can ask for a rematch at the end.
- Some strict networks block direct connections (no TURN relay yet).

**Uploading to itch.io:** zip `index.html` on its own, set Kind of project to HTML, tick "played in the browser", and use a viewport around 1100 × 900 (or enable fullscreen).

## Saving and audio
- The single player run **autosaves** to localStorage after every action. **Export save** gives a `SCR1:` text string; **Import save** loads one. Duels are not saved.
- Your name, mute and music settings are remembered.
- Audio is procedural WebAudio: dice clatter, hits, shield clinks, heal chimes, mana sparkles, win/lose jingles, and a soft tavern loop (🎵 toggles music, 🔊 mutes everything).

## Dev tools
Add `?dev` to the URL for **Win round**, **Lose round** and **+5 ✦** buttons in the top bar (solo and host only). `window.SCR` exposes the engine (`act`, `aiNext`, `newMatch`) for scripted playtests.

## Design decisions
- **One engine, three drivers** (solo AI, host, guest), so single player tests the exact multiplayer rules (pillar 3, and it's what the solo mode is for).
- **Solo opponents are AI hero parties** using the full duel rules (user's choice), fought as 5 waves with a reward after each (user's request, matching Slice & Dice).
- **Solo party: pick 4 of 8** (user's choice). **Theme cards included**, one per match (user's choice).
- **Mana is added when you finish rolling**, so you can spend it the same turn. Simpler than waiting for the resolve step (pillar 1).
- **Default targets** for every die, so a simple turn is roll → commit; targeting is an override (pillar 1).
- **Incoming badges** on every hero and life box show planned damage, shields and heals, so the responder sees exactly what's coming (pillar 2).
- **Draws** (both sides fall on the same turn) replay the round without rewards.

## Assumptions made in this prototype
These were filled in without a direct decision; change any of them freely.
- Hero roster (12), spells, items, relics and themes and all their numbers. Rogue was given a ✦1 face so it can use Backstab.
- Solo: rivals get as many random upgrades as you have; the rival rolls first every wave; a loss ends the run with a retry option.
- Duel: round 1 is started by the player who picked second in the draft.
- Spells can't be cast by a downed hero; one cast per spell per turn.
- Logo and favicon (die with a red chop slash) were drawn without a concept round.

## To review later
- **Rounds are short.** In 30 AI-vs-AI test matches, a round usually ended in 2-4 turns: 4 dice deal about 8 damage per turn against 12 life. If it feels too quick, the levers are life totals, shields or rerolls.
- AI keeps dice by simple face value and greedily kills heroes it can finish; it's a fair practice partner but not a strong one.
- The host's tab must stay in front (background tabs slow down timers).

## Roadmap to alpha
- More heroes, spells and items; hero-specific face types (poison, cleave, pierce shields).
- A per-round theme option, and a theme draft.
- Spectators and best-of-3 / best-of-7 options.
- Turn timer for duels; reconnect if a guest drops.
- An in-game wiki.
