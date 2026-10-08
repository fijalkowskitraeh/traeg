# Styles, rarities, rolls and Awakenings

The design for every ability ("style") in the 48-player tournament: balance, rarities and their drop rates, what is in each rarity, the reroll system, and the plan for better Awakening animations.

On the plan page (`plan/index.html`):

- **🧍 Styles** opens the 3D viewer with all 20 styles, their ability and Awakening, mode notes, counterplay and drop chance.
- **🎲 Roll** opens a working demo of the roll: odds table, guarantee counters, Keep new / Keep current, duplicates, history and a "Simulate 1,000 rolls" check.
- The data lives in one block in the viewer script (search for `const CLS=` and `const CH=[`).

## Contents

1. [At a glance](#1-at-a-glance)
2. [What was wrong with the old roster](#2-what-was-wrong-with-the-old-roster)
3. [How the three proposals were combined](#3-how-the-three-proposals-were-combined)
4. [Design pillars and global rules](#4-design-pillars-and-global-rules)
5. [Rarities and drop rates](#5-rarities-and-drop-rates)
6. [What is in each rarity](#6-what-is-in-each-rarity)
7. [Every style in full](#7-every-style-in-full)
8. [Rolling](#8-rolling)
9. [Guarantees (pity)](#9-guarantees-pity)
10. [Duplicates and the Style Book](#10-duplicates-and-the-style-book)
11. [Coins: how players earn rolls](#11-coins-how-players-earn-rolls)
12. [Robux and the rules for paid random items](#12-robux-and-the-rules-for-paid-random-items)
13. [Awakening animation upgrade](#13-awakening-animation-upgrade)
14. [What changed on the plan page](#14-what-changed-on-the-plan-page)
15. [Open questions](#15-open-questions)

## 1. At a glance

**Rarer means flashier, more complex and more situational, never stronger.** A longer cooldown pays for a bigger effect, so every rarity has the same power per minute. Every Awakening has the same budget: about one strong chance created or denied, lasting 4–6 s, with a named counter.

| Rarity | Styles | Chance per roll | Each style | With guarantees | Cooldown | In a 48-player lobby |
|---|---|---|---|---|---|---|
| Common | 7 | **52%** | 7.43% | 50.3% | 12–14 s | ~25 |
| Rare | 5 | **28%** | 5.6% | 27.1% | 16–18 s | ~13 |
| Epic | 4 | **14%** | 3.5% | 15.6% | 20–22 s | ~7 |
| Legendary | 3 | **5%** | 1.67% | 5.7% | 24–26 s | ~2–3 |
| Mythic | 1 | **1%** | 1% | 1.3% | 28–30 s | ~0.5 |
| **Total** | **20** | **100%** | | | | |

"With guarantees" is what players really get once the guarantees are counted (Epic or better by roll 10, Legendary or better by 50, the Mythic by 200; from a 2-million-roll simulation). On average a player gets an Epic or better every ~4.4 rolls, a Legendary or better every ~14 and the Mythic every ~79. "In a 48-player lobby" assumes every player holds one random roll; bots use the same table without the Mythic.

| Rarity | Styles (formerly) |
|---|---|
| Common | Burst (Zryw), Swerve (Łuk), Cannon (Armata), Tower (Wieża), Playmaker (Rozgrywający), Wolf (Wilk), Ballhawk (new) |
| Rare | Groove (Taniec), Acrobat (Akrobata), Glacier (Lodowiec), Ember (Feniks), Snapshot (Impakt) |
| Epic | Monarch (Król), Bulwark (Ściana), Illusionist (Iluzjonista), Shade (Cień) |
| Legendary | Tempest (Grom), Mimic (Kameleon), Analyst (Analityk) |
| Mythic | Hourglass (Metawizja) |

Aksamitne przyjęcie stays out: receiving the ball must work the same for everyone.

## 2. What was wrong with the old roster

The old character viewer showed **14 styles: 1 Common, 4 Rare, 6 Epic, 3 Legendary**. Five drafts were hidden (Armata, Łuk, Rozgrywający, Wieża, Analityk).

### Structural problems

1. **The pyramid was upside down.** With 1 Common and 6 Epics, any drop table either makes Wilk half the server or makes Epic the tier you see most.
2. **"The stronger the character, the longer the cooldown" admitted that Legendaries were stronger.** Once styles come from rolls, that is luck-to-win, and pay-to-win if Robux is involved. Aura charges at the same rate for every tier, so a stronger Legendary Awakening also fired just as often as a Common one.
3. **Six Awakenings could win a match on their own:** Metawizja, Król, Ściana, Grom, Kameleon, and Cień in Match 5. That broke the page's own rule that abilities "don't win a match on their own".
4. **Numbers were missing:** Zryw's Awakening speed, Akrobata's "much higher" and "much farther", Ściana's reach and wall size, Iluzjonista's number of fakes, Kameleon's range and copy strength.
5. **"Next action" buffs had no time window** (Impakt, Armata, Łuk, Rozgrywający, Wieża), so a player could hold them forever.
6. **Keys 1/2/3 promised three abilities**, but every style had exactly one.
7. **Text and viewer disagreed.** Cień's text said 2 s, but the viewer played 2.6 s. The counter started at "1 / 15" while 14 styles were shown. Instant effects (Impakt, Cień's teleport, Kameleon's copy) played as 6 s Awakenings.
8. **Readability.**
   - Every style used the same telegraph (26 converging cubes plus a hexagon), so rivals couldn't tell which Awakening was coming.
   - Colours collided: Impakt and Grom shared #FFD23F, Cień and Kameleon were both purple, Taniec and Akrobata used the same gold-and-pink pair, and Łuk and Rozgrywający were both light cyan.
   - Nobody could see who had 100 aura.
9. **There were no stacking or crowd-control rules.** Zryw plus the speed crate reached +105%. Wilk's root, Grom's stuns, Król's push and the freeze crate could chain.
10. **Aura had loops and gaps.**
    - A beaten slide gave +10 aura, and Taniec made that easy to farm.
    - You could earn aura during your own Awakening.
    - Hot Potato and Match 5 had almost no aura sources.
    - Carry-over between matches was undecided.

### Style by style

| Style (tier, cooldown) | Main problem |
|---|---|
| Zryw (R, 20 s) | +75% speed is a free breakaway every 20 s. The 7 s Awakening had no number. |
| Taniec (R, 18 s) | The Awakening removed the dribble cooldown and added an auto-dodge, so you were untackleable for 6 s and it farmed its own aura. |
| Metawizja (L, 28 s) | The Awakening slowed the **whole pitch**, keeper included, to 40% for 5 s: a near-certain goal. |
| Król (E, 24 s) | The Awakening shoved everyone, shot 2× faster and passed through blockers, perhaps keepers too. |
| Kameleon (L, 30 s) | Copied any Awakening at full strength, even one the rival hadn't earned. |
| Akrobata (R, 18 s) | No numbers. Overlapped Metawizja (volleys) and Wieża (jumps). |
| Ściana (E, 22 s) | A moving wall that stopped every shot. Parked on the goal line, it was a 5 s shutout. |
| Impakt (E, 22 s) | Too weak for Epic: 100 aura only saved 1 s of charging. Overlapped Armata. |
| Cień (E, 25 s) | The teleport had no range. In Match 5, "behind the defender" could mean behind the AI keeper. |
| Wilk (C, 14 s) | Rooted everyone within 8 m. The strongest crowd control in the game sat on the cheapest tier. |
| Lodowiec (R, 19 s) | A 10 m moving no-sprint zone covered a third of a street pitch. Useless in hockey. |
| Iluzjonista (E, 23 s) | Unlimited fakes for 6 s turned every shot into a coin flip. The pass ability did nothing in 3 of the 7 games. |
| Feniks (E, 24 s) | The ability was Rare level. The Awakening undid a correct tackle, and in Hot Potato it handed the potato back to you. |
| Grom (L, 29 s) | Three automatic, unwarned turnovers with 0.5 s stuns. |
| Drafts | Rozgrywający's uninterceptable pass and Analityk's 6 s silence were hard counters. Armata, Łuk and Wieża only needed numbers. |

### Mode interactions

- **Hot Potato:**
  - Wilk's root set up a free elimination.
  - Feniks handed the potato back to you, and Ściana made you immune to it.
  - The pass styles did nothing.
  - A 28–30 s Legendary cooldown meant 0–1 uses per 30 s round.
  - Anything fired at 0:02 decided the round.
- **Match 5 (AI keeper):** Metawizja slowed the keeper, Król's shot could pass through it, Cień could land behind it, and Iluzjonista made it guess 50/50.
- **Final 1v1:** team effects were wasted, and every stun or slow hit the only rival.
- **Match 3:** hockey made Lodowiec pointless, basketball over-rewarded jump and header styles, and crates stacked with abilities that boost the same stat.

### Originality

All names and visuals must be original. These were renamed:

- **Metawizja** is a signature technique name from a famous soccer anime, so it becomes **Hourglass**.
- **Kameleon** echoes that anime's copy player, so it becomes **Mimic**.
- **Król:** "King" is a striker's nickname in the same anime, so it becomes **Monarch**.
- **Feniks** is a fire hero with a rebirth ultimate, like a famous shooter character, so it becomes **Ember**.
- **Impakt** becomes **Snapshot**, a plain football term, so it can't be read as a famous rival's signature shot.
- Also avoided: Rampart and Bastion (famous shooter characters who build walls), Royal Guard (a famous action-game style), Shadowstep (a famous MMO skill), Time Stop, Echo, Storm and Hawkeye.

## 3. How the three proposals were combined

Three independent proposals were written: competitive integrity, economy and rolls, and fun and spectacle.

- **Competitive integrity** gave the global fairness rules and the exact numbers for every style.
- **Economy and rolls** gave the five-tier pyramid, drop rates, guarantees, coin income, slots, duplicates and Robux compliance.
- **Fun and spectacle** gave the names and fantasy, the presentation budget per tier, the shared animation kit and most Awakening briefs.

| Conflict | Competitive / Economy / Spectacle | Decision |
|---|---|---|
| Roster and tiers | 19, no Mythic / 20 as 7-5-4-3-1 / 19 as 7-5-4-2-1 | **20 styles, 7-5-4-3-1** (economy) |
| Mythic tier | no / yes / yes | **Yes, one style.** Its numbers sit on the Legendary budget, and bots never get it. |
| Drop rates | 52-27-15-6 / 52-28-14-5-1 / 56-25-14-4-1 | **52 / 28 / 14 / 5 / 1** (economy) |
| Cooldown bands | compressed / compressed / wider | **12–14, 16–18, 20–22, 24–26, 28–30 s** |
| Keys 1/2/3 | 3 styles with a shared cooldown / one style / one style | **One active style:** 1 = ability, G = Awakening, 2 = crate item, 3 = comms wheel |
| Switching style | locked / between matches / locked | **Locked for the whole tournament**, so styles stay readable and nobody counter-picks |
| Roll model | permanent collection / keep or replace a slot / keep or discard | **Keep new or keep current, per slot.** The Style Book records everything you roll. The collection model is an open question. |
| Coins | 100 per roll, 500 start / 100, 300 plus a free roll / 50, 150 | **100 per roll, 300 at the start plus a free Rare-or-better roll** (economy), with the competitive anti-farm rules |
| Guarantees | Epic 10, Legendary 40 / Epic 10, Legendary 50, Mythic 200 / Epic 15, Legendary 50, Mythic 100 | **Epic or better by 10, Legendary or better by 50, Mythic by 200** (economy) |
| Robux | none at launch / paid spins with full compliance / none at launch | **No paid random spins at launch.** Robux buys specific styles and cosmetics. The economy's compliance spec is ready if spins are added later. |
| Speed cap | +35% / +45% / +60% | **+35% total** (competitive) |
| Forbidden effects | banned / partly / partly | **Banned:** pass-through or unblockable shots, automatic return of a lost ball, full invisibility, silences, automatic dodges and steals |
| Area size | 7 m cap / 6–9 m / 7–12 m | **7 m cap** (4 m in Hot Potato) |
| AI keeper | immune / immune / slowed 20% by time effects | **Immune to every style effect** |
| Hot Potato Awakenings | off / locked in the last 5 s / on | **On, but they can't be started in the last 10 s**, so every Awakening ends with at least 4 s left |
| Aura carry-over | reset, Final starts at 50 / carry up to 50 / keep 50%, up to 50 | **Carry up to 50**, and both finalists start the Final with at least 50 |
| Cannon vs Impakt | separate / separate / merged | **Separate:** Cannon (Common, how hard you kick) and Snapshot (Rare, how fast you release) |
| New styles | none / Ballhawk / Hawk and Ricochet | **Ballhawk**, with passive reach and no automatic steal. Ricochet waits for a later season: it depends on the arena, its homing bend clashes with the no-automatic-effects rule, and 20 styles is the cap. |
| Analyst | information-only Epic / Legendary with an ability lock / retired | **Information-only Legendary.** Its "who is ready" idea becomes a global ready glow. |
| Ściana's wall | static, in front / behind, 2 blocks / behind, 3 blocks | **Behind you and following you** (the original fantasy), with 3 HP, lobs over it, the 8 m goal rule and a 15% slow |
| Cień's teleport | free 7 m blink / behind the nearest rival / through the nearest rival | **An aimed 7 m blink that may pass a defender.** No stun on them, a 0.5 s no-shot lag, a decoy and no landing in the goal area. |
| Mimic's Awakening | last one fired / nearest rival, even uncharged / the rival you face | **The rival you face, warned 1 s ahead, at 75% duration.** With equal Awakening budgets, copying is a sidegrade. |
| Taniec's auto-pirouette | removed / kept once / kept once | **Removed.** The pirouette now plays when you beat a slide yourself. |
| Feniks's ball return | loose ball / loose ball / back to your feet | **Loose ball.** The tackler keeps the reward. |

## 4. Design pillars and global rules

Strength per use rises with the cooldown, so power per minute is the same in every tier. The reference unit is Burst: +30% speed for 3 s every 13 s.

| Tier | Cooldown | Value per use (Common = 1.0) | Uses in a 3-min match | Value per match |
|---|---|---|---|---|
| Common | 12–14 s | 1.0 | ~13 | ~13 |
| Rare | 16–18 s | 1.3 | ~10.5 | ~13.7 |
| Epic | 20–22 s | 1.6 | ~8.5 | ~13.6 |
| Legendary | 24–26 s | 1.9 | ~7.2 | ~13.7 |
| Mythic | 28–30 s | 2.2 | ~6.2 | ~13.6 |

**Global rules** (also on the plan page, in Rules sections 11–12 and in Controls):

1. **One style per player:** 1 = ability, G = Awakening, 2 = crate item, 3 = comms wheel.
2. **Caps:**
   - Movement bonus: +35% in total from all sources.
   - Slows: at most 40%.
   - Shot speed: at most 130% of a normal full-power shot.
   - Jump: at most +50%.
   - The same stat from different sources (styles, crates, teammates) doesn't stack; the highest applies.
3. **Crowd control:** a stagger lasts at most 0.5 s. Any stagger, shove, slow or startle gives 2 s of immunity, shown as a white outline. Goalkeepers are immune to every style effect: the AI keeper, and players inside their own goal area.
4. **Forbidden effects:**
   - unblockable or pass-through shots;
   - automatic return of a lost ball;
   - full invisibility (the ball is always visible);
   - silences or ability locks;
   - automatic dodges or steals.
5. **Areas:** a radius of 7 m at most (4 m in Hot Potato). Nothing affects the whole pitch.
6. **"Next action" buffs** expire after the window in their text. The cooldown starts when the ability is fired.
7. **Telegraphs:**
   - Each style has its own silhouette, colour and sound, plus its icon over your head and a ground ring.
   - You move at 50% speed during your 1 s Awakening telegraph.
   - Being tackled doesn't cancel a telegraph, and you can't start one while staggered.
8. **Ready glow:** at 100 aura you get a rim light in your style colour and an icon over your head, so every rival knows an Awakening is loaded.
9. **Aura:**
   - No aura is earned during your own Awakening.
   - Actions an Awakening causes on its own (wall blocks, lightning turnovers) give no aura.
   - Dribble aura is capped at 20 per minute.
   - Up to 50 aura carries into the next match, and both finalists start the Final with at least 50.
10. **Hot Potato:**
    - All cooldowns reset at the start of each round.
    - Awakenings can't be started in the last 10 s.
    - Aim assist is off for bent kicks.
    - Only the potato touching you makes you the holder, so effects that would steal the ball become deflections.
    - The round clock never slows.
    - Aura: +10 for kicking the potato into a rival, +5 for a dodge (the potato passes within 1 m) and +10 for surviving a round, max 30 per round.
11. **Dribble guard:** only the first 0.35 s of a stepover or rainbow beats a slide.
12. **Crates** follow the same caps. The freeze crate becomes a 0.5 s stagger.
13. **Cosmetics** never change a telegraph's shape, a range ring or any other gameplay tell.
14. **Presentation is client-side:** hit-stop, slow motion, flashes and camera moves play only on each viewer's own camera, and server physics never pauses. Reduced motion turns off shake, hit-stop and FOV punches and halves the particles.

## 5. Rarities and drop rates

When styles are added later, each rarity keeps its rate and only the share per style changes.

| Rarity | Colour | Chance per roll | Styles | Each style | Cooldown | Duplicate refund |
|---|---|---|---|---|---|---|
| Common | `#8FA3B8` | **52%** | 7 | 7.43% | 12–14 s | 25 coins |
| Rare | `#3D8BFF` | **28%** | 5 | 5.6% | 16–18 s | 40 coins |
| Epic | `#B26BFF` | **14%** | 4 | 3.5% | 20–22 s | 60 coins |
| Legendary | `#FFB21A` | **5%** | 3 | 1.67% | 24–26 s | 100 coins |
| Mythic | `#FF3B6B` | **1%** | 1 | 1% | 28–30 s | 200 coins |

- **Common (52%):** One clear number or one simple trick you can explain in a sentence, easy for both sides to read. It has the shortest cooldown, so it adds up to the same power per minute as any other tier through many small uses (about 13 per 3-minute match). It is the largest tier (7 styles) and covers every role (speed, curve, power, headers, passing, pressing, interception), so new players get variety and few duplicates. Presentation: Awakening effects stay within about 3 m of the player plus a trail, with a local flash, 0.05 s hit-stop and a 4° camera punch.
- **Rare (28%):** One twist on a basic action (dribble rhythm, jumps, ice, stamina insurance, instant release) that rewards timing and positioning more than raw reaction. Each use is a bigger moment on a 16–18 s cooldown, at the same value per minute. Presentation: areas up to the 7 m cap, a two-colour palette, a 25% screen flash, a name banner, 0.06 s hit-stop and a 6° punch.
- **Epic (14%):** Acts directly on rivals or on the ball's path (shield and shove, a wall, fakes, smoke and blink), always with a limit rivals can see: HP gems, hologram counters, a range ring. This creates mind games, but every big moment can be beaten. 20–22 s cooldowns. Presentation: a summoned set piece (crown, fortress, top hat, seal), a 40% flash, 0.08 s hit-stop, an 8° punch and a music sting.
- **Legendary (5%):** Complex, high-skill styles that read or borrow from other players, or pressure the space around the ball carrier with telegraphed strikes. High ceiling, low floor: never stronger than a Common per minute, just deeper. 24–26 s cooldowns. Presentation: the sky and lighting change, letterbox bars, a 60% flash, 0.12 s hit-stop and a 10° punch.
- **Mythic (1%):** A single chase style at 1%, with the most spectacular presentation and a server-wide announcement when someone rolls it. Its numbers sit on the Legendary budget: Mythic buys spectacle, identity and bragging rights, never extra power. It has its own guarantee (200 rolls), is sold for coins only, and bots never get it. Presentation: the whole scene transforms, with a 0.2 s hit-stop and a slow-motion ramp on each viewer's own camera, a 12° punch, letterbox bars and an announcer line.

## 6. What is in each rarity

### Common (52%): one clear number, every role covered

| Style (was) | Role | Cooldown | Ability | Awakening | Chance |
|---|---|---|---|---|---|
| [Burst](#burst) (Zryw) | speed | 13 s | +30% speed for 3 s (+20% with the ball) | Redline: 5 s at the +35% cap, free sprint | 7.43% |
| [Swerve](#swerve) (Łuk) | curve | 13 s | next shot curves up to 2× | Double Bend: one S-shaped shot | 7.43% |
| [Cannon](#cannon) (Armata) | shot power | 14 s | overcharge the next shot to 120% | Siege Shot: one 130% shot with no drag | 7.43% |
| [Tower](#tower) (Wieża) | headers | 13 s | headers +30%, contact 0.3 m higher | Tower Leap: one jump 50% higher, header like a shot | 7.43% |
| [Playmaker](#playmaker) (Rozgrywający) | passing | 12 s | next pass +25%, led into the run | Conductor: faster passes, receivers' next shot +15% | 7.43% |
| [Wolf](#wolf) (Wilk) | pressing | 14 s | trail to the ball carrier, +15% toward them | Howl: 7 m startle, pack +15% toward the ball | 7.43% |
| [Ballhawk](#ballhawk) (new) | interception | 13 s | interception reach +1 m, see pass lines | Talon: reach +2 m, speed burst after each interception | 7.43% |

### Rare (28%): one twist that rewards timing

| Style (was) | Role | Cooldown | Ability | Awakening | Chance |
|---|---|---|---|---|---|
| [Groove](#groove) (Taniec) | dribbling | 17 s | stepover and rainbow cooldowns −40% for 5 s | Showtime: −60% cooldowns, free dribbles, speed after beating a slide | 5.6% |
| [Acrobat](#acrobat) (Akrobata) | volleys and bicycle kicks | 16 s | jump +35% for 4 s | Big Top: volleys and bicycle kicks +25%, longer hit window | 5.6% |
| [Glacier](#glacier) (Lodowiec) | ice and slide | 17 s | next slide leaves a 6 m ice strip | Whiteout: a 5 m ice ring follows you | 5.6% |
| [Ember](#ember) (Feniks) | stamina and fire | 18 s | +35 stamina, regen while sprinting | Rekindle: free stamina, and the first tackle on you becomes a loose ball | 5.6% |
| [Snapshot](#snapshot) (Impakt) | quick shot | 16 s | shots charge 2× faster for 4 s | Sound Barrier: 5 s of instant full-power shots | 5.6% |

### Epic (14%): acts on rivals or the ball, always with a visible limit

| Style (was) | Role | Cooldown | Ability | Awakening | Chance |
|---|---|---|---|---|---|
| [Monarch](#monarch) (Król) | strength | 21 s | front slides bounce off for 2.5 s | Coronation: each close rival shoved once, +30% shot that can be deflected | 3.5% |
| [Bulwark](#bulwark) (Ściana) | defense | 21 s | tackle and block reach +40% | Fortress: a 3 HP wall follows behind you | 3.5% |
| [Illusionist](#illusionist) (Iluzjonista) | tricks | 20 s | next pass or shot bends around one rival | Grand Finale: 2 hologram balls with a shadow tell | 3.5% |
| [Shade](#shade) (Cień) | stealth | 22 s | 4 m smoke cloud; the ball stays visible | Vanishing Cut: an aimed 7 m blink past a defender | 3.5% |

### Legendary (5%): high skill ceiling, reads or borrows from others

| Style (was) | Role | Cooldown | Ability | Awakening | Chance |
|---|---|---|---|---|---|
| [Tempest](#tempest) (Grom) | storm tackling | 24 s | next slide goes 60% farther | Thunderhead: 3 telegraphed strikes on the ball carrier | 1.67% |
| [Mimic](#mimic) (Kameleon) | copying | 26 s | copy the nearest rival's ability at 80% | Stolen Shadow: fire the faced rival's Awakening at 75% | 1.67% |
| [Analyst](#analyst) (Analityk) | tactics | 25 s | read rivals' shot lines and cooldowns | Game Plan: your team sees every rival shot line and pass direction | 1.67% |

### Mythic (1%): one chase style, same budget, the biggest show

| Style (was) | Role | Cooldown | Ability | Awakening | Chance |
|---|---|---|---|---|---|
| [Hourglass](#hourglass) (Metawizja) | time | 29 s | the airborne ball near you slows to 70% | Still Hour: a fixed 7 m time dome at 70% | 1% |

## 7. Every style in full

Telegraphs are fixed: 0.5 s before an ability and 1 s before an Awakening, always visible to rivals. "Lasts" is the effect window; a "next action" buff expires at the end of it.

### Common styles

<a id="burst"></a>
#### Burst  ·  Common  ·  speed

| Cooldown | Ability lasts | Awakening lasts | Chance per roll | Colours | 3D effect | Formerly |
|---|---|---|---|---|---|---|
| 13 s | 3 s | 5 s | 7.43% | `#FF3B3B` `#FFB0A0` | `speed` | Zryw |

- **Ability (1):** Burst: for 3 s you run 30% faster (20% faster with the ball). Stamina use is unchanged. Telegraph (0.5 s, rivals can see it): red sparks gather around your boots. Animation: red rings pop out of the turf with every stride, and short wind streaks whip past your legs.
- **Awakening (G, 100 aura):** Redline, 5 s: you run 35% faster (25% faster with the ball), which is the global speed cap, and sprinting costs no stamina. Slide tackles and blocks work on you as normal. Telegraph (1 s, rivals can see it): you drop into a sprinter's crouch, a red ring tightens around your feet and sparks are sucked into your legs. Animation: a red shockwave and a burst of dust at launch, then you run inside a tunnel of light streaks, with red lightning crawling over your body, a red flame aura, three afterimages and a glowing track burned into the turf behind you.
- **In each mode:** Team matches and Bomb: as written. Hot Potato: as written. Cooldowns reset each round, so you get about 2 uses per round; it helps you escape or chase, and the small with-ball bonus means the holder can't simply run someone down. 1v1v1v1 (AI keeper): wins races to loose balls; the keeper is unaffected. Final: as written; a defender who is already goal-side keeps up. Basketball and volleyball: no jump change; it helps you reach the gold landing ring first. Hockey: higher top speed but no extra grip, so you overshoot turns. Crates: the speed crate doesn't stack (the higher bonus applies, capped at +35%).
- **Counterplay:** The 1 s crouch is the cue to drop off instead of stepping in. With the ball the bonus is only 20–25%, so a defender who is goal-side still meets them. There is no tackle protection, so a timed slide wins the ball. Or simply wait out the 3 s.
- **Balance:** This is the reference unit of the whole roster: +30% for 3 s every 13 s (23% uptime) = value 1.0. Speed is the most universally useful stat, so it gets the smallest number in the Common tier. The with-ball bonus is lower because speed with the ball is what beats defenders. Redline sits exactly at the +35% cap, so nothing can stack on top of it.
- **What changed:** Rare → Common: the simplest effect makes the ideal starter. +75% → +30% (+20% with the ball); cooldown 20 → 13 s. Awakening: 7 s of unnumbered 'huge speed' (the longest Awakening on the roster) → 5 s at +35% (+25% with the ball), free stamina kept. Renamed Zryw → Burst, and the Awakening is named Redline.

<details><summary>Awakening animation brief</summary>

Existing 'speed' fx, Common budget (about 3 m plus the trail, 0.05 s hit-stop). TELEGRAPH 0–1 s in three heartbeat beats (0.33/0.66/1.0 s). Each beat lights one of three tachometer segments (thin emissive boxes) on each shin, pulses the hexagon glyph tighter (scale 3.2 → 1.0), bumps the camera 0.02 and plays a low thump. The converging cubes, tinted red, spiral into the calves with a red-to-white ramp. The camera drops to knee height behind the heels and FOV narrows 36 → 31. At 0.85 s the legs tremble (±0.02 jitter) and the dust ring is sucked inward (reversed particle velocity). BURST at 1.0 s: 0.05 s hit-stop; a 2-frame white flash sprite at the feet (billboard plane, scale 0.2 → 4 in 0.12 s); the shockwave ring grows 0 → 7 m in 0.25 s; 24 dust cubes kick backward under gravity; two dark skid decals mark where the feet pushed off; FOV snaps wide 31 → 46 in 0.12 s and eases back over 0.4 s (wide reads as speed). LOOP: warp streaks scale with run-cycle speed. Replace the static road plane with a ribbon trail (a BufferGeometry strip built from the last 30 foot positions, with a fading tail and 1 s life). Three afterimages lag 0.06 s apart, tinted red, orange and white for a chromatic smear. Flame cones flicker at 12 Hz. Lightning re-rolls only every 0.6 s and on turns, so it reads. Every 1.2 s a 'gear shift': a red ring peels off the back with an FOV bump of +2°. A red countdown ring drains on the ground. OUTRO (last 0.6 s): the tach segments switch off top-down, the heels dig in and 20 sparks spray forward, the streaks slow and fade, FOV eases to 36 (expo-out), the afterimages catch up and merge into the body with a small red pop, and three grey steam puffs rise from the shoes. Hide the generic shell cones. Palette: #FF2A1A core, #FFB070 rim, white only at peaks. Reduced motion: no FOV snap, shake or hit-stop, and half the particles. Budget about 110 meshes.

</details>

<a id="swerve"></a>
#### Swerve  ·  Common  ·  curve

| Cooldown | Ability lasts | Awakening lasts | Chance per roll | Colours | 3D effect | Formerly |
|---|---|---|---|---|---|---|
| 13 s | 4 s | 5 s | 7.43% | `#12D6C4` `#B8FFF6` | `swerve` (to build) | Łuk |

- **Ability (1):** Bend: your next shot or lob within 4 s can curve up to twice as much as normal, and your shot line shows the full bend. As with normal curve, a strongly bent ball loses up to 10% speed. Telegraph (0.5 s, rivals can see it): a teal spiral wraps around your kicking boot. Animation: the ball spins with a teal stripe and leaves a curved teal ribbon in the air that hangs for 1 s, so everyone can read its path.
- **Awakening (G, 100 aura):** Double Bend, 5 s: your next shot or lob within 5 s bends twice. It curves one way, then after 60% of its flight it swaps direction once; you set the second bend by moving the mouse the other way while charging. The ball leaves a teal trail everyone can see. Telegraph (1 s, rivals can see it): two teal arcs draw an S on the ground in front of you, and the ball spins in place at your feet. Animation: the ball flies inside a corkscrew of light, flashes with a ring of sparks at the turn point and snaps into the second bend, leaving an S-shaped glowing trail in the air.
- **In each mode:** Team matches, Hockey and Bomb: as written. Hot Potato: aim assist is off for bent kicks, so the two don't add up; the bend lets you hit a rival hiding behind another player. 1v1v1v1 and Final: the AI keeper re-reads the ball at the turn point with its normal reaction time, so it saves fewer of these shots, but never none. Basketball: bends work on hoop shots too, but the hoop is small. Volleyball: the gold landing ring always shows the real landing spot. Crates: no interaction.
- **Counterplay:** Block at the source: curve doesn't help against a body in front of the boot. The spiral telegraph and the visible trail show the side early. The second bend always goes back the other way, so a keeper who stays central and doesn't dive early saves it. Curved balls are slower.
- **Balance:** Curve is a skill multiplier with a built-in speed tax (value about 1.0). The Awakening is one tricky shot with a visible tell, which fits the shared Awakening budget. The manual second bend means there is no auto-aim.
- **What changed:** Hidden draft Łuk finished and renamed Swerve. Added the 4 s window, the speed tax, the visible trail and a manually set second bend. Cooldown 15 → 13 s. New teal colour, so it no longer clashes with Playmaker's light cyan. Needs a new fx.

<details><summary>Awakening animation brief</summary>

New fx 'swerve', Common budget. BUILD: two TubeGeometry ribbons on a CatmullRomCurve3 helix around the kicking leg (animate with geometry.setDrawRange); a ball with a teal stripe from a CanvasTexture; a reusable flight ribbon (TubeGeometry along the S-path, its drawRange following the ball); a goal frame of thin boxes 8 m out; a keeper dummy from the existing dummy builder. TELEGRAPH 0–1 s: 0–0.4 s two teal ground arcs draw themselves as an S from the feet to 4 m ahead. 0.4–0.8 s the leg ribbons climb from boot to knee and 12 teal chips spiral around the boot; the ball spins in place, ramping 0 → 30 rad/s, with 8 wind motes orbiting it. 0.8–1.0 s the S pulses twice with a rising whistle, while the camera lowers to ball height and swings 60° to a side view so the S will read. BURST at 1.0 s: leg whip, 0.05 s hit-stop on contact, a teal flash sprite at the ball and an FOV punch of −4° easing back over 0.25 s; the ball launches along the S (curve.getPointAt). At the 60% turn point: 0.05 s hit-stop, a teal ring snaps open perpendicular to the flight with 16 sparks, the ribbon colour flips teal → white, and the camera nudges 0.1 toward the break side. LOOP: a new shot every 2.2 s, alternating left and right S-curves; each double ribbon lingers 0.6 s; the keeper dummy dives toward the first bend and is wrong-footed at the flip; the net flashes where the ball goes in; a teal countdown ring drains. OUTRO (last 0.5 s): the last ribbon dissolves into teal dust drifting upward, and the leg spirals unwind down into the turf. Palette #12D6C4, #B8FFF6, white. Budget about 80 meshes.

</details>

<a id="cannon"></a>
#### Cannon  ·  Common  ·  shot power

| Cooldown | Ability lasts | Awakening lasts | Chance per roll | Colours | 3D effect | Formerly |
|---|---|---|---|---|---|---|
| 14 s | 4 s | 5 s | 7.43% | `#FF8A1F` `#FFD08A` | `cannon` (to build) | Armata |

- **Ability (1):** Heavy Boot: your next shot within 4 s can be overcharged. Keep holding 0.4 s past full power to reach 120% power: the ball is 20% faster and flies flatter. The power bar shows an extra red segment, and releasing earlier gives a normal shot. You can't curve while overcharging. Telegraph (0.5 s, rivals can see it): you stamp once and your boot turns iron-grey with glowing orange seams. Animation: heat haze shimmers around the boot, smoke curls from the sole, and an overcharged shot leaves a muzzle flash at your foot and a short orange tracer.
- **Awakening (G, 100 aura):** Siege Shot, 5 s: your next shot within 5 s reaches 130% power (the global shot cap) and keeps full speed over any distance (no air drag). A field player who body-blocks it is knocked back 1 m, but the block still stops the ball. Telegraph (1 s, rivals can see it): you plant your standing foot, the ground cracks in a ring, and hoops of orange light lock around your shin like a cannon barrel. Animation: the kick fires with a recoil blast, a muzzle flash and a rolling smoke ring; the ball flies as a glowing cannonball with a white-hot core and a long smoke trail, and a blocker is shoved back in a shower of embers.
- **In each mode:** Team matches, Hockey and Bomb: as written. Hot Potato: overcharge caps at 110% and Siege Shot at 115%, with no knockback (the chamber is small). 1v1v1v1 and Final: keepers save 130% shots at their normal rate, minus the usual distance difficulty, and are never knocked back. Basketball: the bonus applies only to ground kicks, not to hoop shots. Volleyball: capped at 110%, because the ball already speeds up with every touch. Crates: doesn't stack with the power-shot crate (the higher value applies).
- **Counterplay:** The overcharge needs 1.4 s of holding, 0.4 s longer than a normal shot, so press the shooter. Siege Shot can still be blocked or saved, and an overcharged shot can't curve. The barrel telegraph gives 1 s to close the lane.
- **Balance:** A sidegrade pair with Snapshot: Cannon is slower and harder, Snapshot is faster and normal. +20% ball speed for 0.4 s more wind-up is a fair trade (value 1.0 on 14 s, the top of the Common band, because it's a direct scoring tool). Siege Shot sits at the 130% cap and stays blockable.
- **What changed:** Hidden draft Armata finished and moved Rare → Common to widen the base of the pyramid. 'Charges stronger' → a 120% overcharge with a 4 s window. 'Shot from any distance with huge power and a shockwave' → 130% with no drag; it stays blockable, and the knockback is only 1 m. Kept separate from Snapshot: Cannon is about how hard you kick, Snapshot about how fast you release. Needs a new fx.

<details><summary>Awakening animation brief</summary>

New fx 'cannon', Common budget. BUILD: up to 6 TorusGeometry hoops (iron #5A5560 with additive orange rims) forming a barrel; a crack-ring decal made of thin planes; a smoke-torus pool; a muzzle-flash cone (additive ConeGeometry); an ember pool via the existing emit/tick helper; a blocker dummy 6 m out. TELEGRAPH 0–1 s: the camera drops to shin height. 0–0.35 s the back foot plants and jagged crack lines spread in a 1.5 m ring. 0.35–0.8 s three hoops slide down the leg and lock with a clank flash at 0.35, 0.55 and 0.8 s. 0.8–1.0 s the hoops light white in sequence like a capacitor charging, with a rising hum, and embers rise from the boot. BURST at 1.0 s (activation): 0.05 s hit-stop, a ground shock ring and an orange flash sprite around the shin. LOOP: the hoops rotate slowly, the boot pulses orange at 2 Hz and a heat-haze plane wobbles behind the leg. At 1.6 s the shot: the figure recoils 0.3 m, the muzzle-flash cone fires for 0.1 s, a smoke torus expands 0.3 → 2.5 m and slows with drag, 12 sparks fire in a forward cone, the camera kicks back 0.3 m then springs forward, and a 0.15 s shake at amplitude 0.08 plays. The ball becomes a cannonball (white-hot core, orange shell) with a 6 m smoke ribbon and heat-shimmer rings every 0.1 s. It hits the blocker dummy, which bursts into 30 embers and slides back exactly 1 m (showing the knockback), while the ball drops dead. An orange countdown ring drains. OUTRO (last 0.6 s): the hoops crack and fall as 6 grey shards that clink and fade, and a last puff of steam rises from the boot. Palette #FF8A1F, #FFD08A, iron #5A5560, white core. Budget about 100 meshes.

</details>

<a id="tower"></a>
#### Tower  ·  Common  ·  headers

| Cooldown | Ability lasts | Awakening lasts | Chance per roll | Colours | 3D effect | Formerly |
|---|---|---|---|---|---|---|
| 13 s | 4 s | 5 s | 7.43% | `#9AA6FF` `#DCE0FF` | `tower` (to build) | Wieża |

- **Ability (1):** Header Pro: for 4 s your headers fly 30% faster and farther, and you can meet the ball 0.3 m higher above your head. Your jump height is unchanged. Telegraph (0.5 s, rivals can see it): you roll your neck, tap your forehead, and a lavender halo flickers above your head. Animation: the halo spins over your head while the ability lasts, and every header flashes a lavender ring and leaves a short lavender streak and a puff of stone dust.
- **Awakening (G, 100 aura):** Tower Leap, 5 s: your next jump within 5 s goes 50% higher (0.3 s wind-up) and you hang for 0.3 s at the top; a header from that jump flies like a full-power shot. After landing you can't jump for 1 s. Telegraph (1 s, rivals can see it): you stamp twice, the ground rumbles, a ring of stone blocks pushes up around your feet and a lavender column of light rises to head height. Animation: a stone pillar erupts under you and lifts you, you hang at the top inside a glowing halo, and the header leaves with a bell-like flash, a lavender comet trail and a ring of stone debris while the pillar crumbles behind you.
- **In each mode:** Team matches, Hockey and Bomb: as written (crosses and corners). Modes without passing (Hot Potato, 1v1v1v1, Final): Header Pro also makes your next lob (E) go 25% higher and land 1 s later, so you can run onto your own lob and head it. Hot Potato: a header counts as a kick (you touch the potato and send it on at once), headers redirect it 30% harder, and the Awakening's hang makes you an easy target. Basketball: headers into the hoop score, so the leap is only 25% higher. Volleyball: headers count as strikes, capped at +20%. Crates: the super-jump crate doesn't stack with Tower Leap (the higher value applies, cap +50%).
- **Counterplay:** Contest the header in the air (the higher contact wins), cut out the cross before it arrives, keep the ball on the ground, or wait out the 1 s landing lockout. The 0.3 s leap wind-up and the stone ring are easy to see.
- **Balance:** The most situational Common (it needs a high ball), so it gets generous numbers inside its niche, which comes to value about 1.0 once the condition is counted. The landing lockout stops chained leaps, and the solo-lob rule keeps it useful in the three modes without crosses.
- **What changed:** Hidden draft Wieża finished and renamed Tower. 'Stronger header with more reach' → +30% header speed and contact 0.3 m higher; the ability no longer adds jump height (that's Acrobat's job). 'Giant jump' → one jump 50% higher with a 0.3 s hang and a 1 s landing lockout. Added the solo-lob rule and the basketball cap. Cooldown 14 → 13 s. Needs a new fx.

<details><summary>Awakening animation brief</summary>

New fx 'tower', Common budget. BUILD: a pillar of 6 stacked BoxGeometry segments (0.9 × 0.5 × 0.9, sandstone #B9A88A, lavender edge glow from a slightly larger additive box); a ring of 12 small blocks; a pool of 30 rubble boxes; a halo (TorusGeometry); a ball that arrives on a cross arc from the side. TELEGRAPH 0–1 s: two stamps at 0.2 s and 0.6 s, each with a ground ring and a 0.03 camera dip. The 12-block ring pushes up around the feet (ease-out-back to 0.4 m) with rising dust motes, a lavender column (open CylinderGeometry) grows to head height, and the figure taps its forehead at 0.8 s. BURST at 1.0 s: the pillar erupts segment by segment, 0.04 s apart, lifting the player 2.5 m; 20 dust puffs burst at the base; the camera tilts up 8–10° to follow, with FOV +4°. At the apex: 0.05 s hit-stop and a 0.3 s hang inside a widening halo; the crossed ball meets the head with a bell-flash ring (1.5 m) and rockets off with a lavender comet trail (12 shrinking spheres) and a vertical stone-dust shock ring facing its flight. LOOP: every 1.8 s a new jump on a new pillar and a new header. Meanwhile the previous pillar crumbles: segments fall with gravity and random spin, puff on landing, and are recycled from the pool. A lavender countdown ring drains. OUTRO (last 0.5 s): a heavy landing stomp with a small shock ring; the rubble sinks into the turf; the halo dims (showing the 1 s jump lockout) and dissolves; the camera eases back down. Palette #9AA6FF, #DCE0FF, sandstone. Budget about 80 meshes.

</details>

<a id="playmaker"></a>
#### Playmaker  ·  Common  ·  passing

| Cooldown | Ability lasts | Awakening lasts | Chance per roll | Colours | 3D effect | Formerly |
|---|---|---|---|---|---|---|
| 12 s | 4 s | 6 s | 7.43% | `#3CCB7F` `#FFFFFF` | `playmaker` (to build) | Rozgrywający |

- **Ability (1):** Through Ball: your next pass or lob within 4 s is 25% faster and is led into your teammate's run, arriving within 0.5 m of where they will be. It can still be intercepted. Telegraph (0.5 s, rivals can see it): you point at a teammate and a chalk line flickers between you. Animation: the pass leaves a straight chalk-white line on the grass and lands with a small mint ring at the receiver's feet.
- **Awakening (G, 100 aura):** Conductor, 6 s: your passes and lobs are 30% faster, and every teammate who receives one is Primed for 3 s: their next shot is 15% stronger. Passes can still be intercepted. Telegraph (1 s, rivals can see it): a baton of light forms in your hand and draws a glowing circle in the air, and a chalk tactics board draws itself on the ground around you. Animation: mint lines link you to your teammates like a pitch diagram, each pass rides a glowing line with a gold thread behind it, and Primed teammates get a mint ring at their feet and glowing boots.
- **In each mode:** 8v8 and 4v4 (Matches 1–3): full value; strongest with a teammate who finishes. Basketball: Primed adds +15% to hoop shots as speed only, not accuracy. Volleyball: a pass is a set, so the receiver's strike gets the bonus. Bomb: a fast pass helps the bomb team score. Hockey: as written. Hot Potato, 1v1v1v1 and Final (no passing): Through Ball works on your next lob or kick (+25% speed, no lead; +15% in Hot Potato because the chamber is small), and Conductor makes your own shots and lobs 15% stronger for 6 s instead.
- **Counterplay:** Every pass can still be intercepted. The pointing telegraph shows who will receive, so mark the receiver instead of the lane. Primed teammates glow, so you know whom to close down.
- **Balance:** Team value: weak alone, strong with a finisher, so the numbers are modest and it has the lowest cooldown (12 s). The solo fallbacks keep it at about value 1.0 in the late rounds. It changes pass speed and aim, never how the ball is received, so the 'receiving is identical' rule holds.
- **What changed:** Hidden draft Rozgrywający finished and renamed Playmaker. 'Faster and more accurate' → +25% and led into the run. The uninterceptable pass is removed (no unblockable effects). Added Primed (+15%) and solo fallbacks for the three modes without passing. New mint colour, so it no longer clashes with Swerve. Needs a new fx.

<details><summary>Awakening animation brief</summary>

New fx 'playmaker', Common budget. BUILD: a teammate dummy 6 m ahead-right and a rival dummy halfway (existing dummy builder); a lane of two overlapping additive CylinderGeometry tubes laid flat; a chevron strip (a plane with a repeating-arrow CanvasTexture; animate texture.offset); chalk lines and X/O marks from thin planes; a baton (a thin box); an air circle (a RingGeometry revealed by growing setDrawRange on its index). TELEGRAPH 0–1 s: 0–0.5 s the baton forms in the hand from 10 converging motes and draws a 1.2 m glowing circle in the air, which drops to the ground as a ring. 0.5–1.0 s chalk lines draw a 4 m tactics board on the ground with X and O marks popping in (marker squeak), and a dashed chalk line draws to the teammate with three mint pips lighting at 0.6, 0.75 and 0.9 s; the teammate gets a mint outline (a BackSide scaled clone). BURST at 1.0 s: contact with 0.05 s hit-stop, a mint flash at the foot and an FOV punch of −4°; the lane ignites from passer to receiver in 0.15 s and the chevrons start scrolling. LOOP: the ball rides the lane with a gold thread trail. The rival dummy lunges and arrives a fraction late: the pass is fast, not untouchable, so show a near-miss spark, never a pass-through. On arrival a 16-sparkle starburst, the receiver's boots ignite mint and a small '+15%' sprite pops. The camera dollies along the lane once per pass (lerp from behind the passer to the midpoint over 0.6 s), repeated every 2.4 s. A mint countdown ring drains. OUTRO (last 0.5 s): the lane reels up from passer to receiver like a ribbon, the chalk board is wiped left to right, the baton shatters into notes of light, and the receiver's glow pulses twice and fades. Palette #3CCB7F, white, gold thread #FFD23F. Budget about 70 meshes.

</details>

<a id="wolf"></a>
#### Wolf  ·  Common  ·  pressing

| Cooldown | Ability lasts | Awakening lasts | Chance per roll | Colours | 3D effect | Formerly |
|---|---|---|---|---|---|---|
| 14 s | 4 s | 5 s | 7.43% | `#7FA8C9` `#DDF3FF` | `wolf` | Wilk |

- **Ability (1):** Scent: for 4 s you see a glowing trail to the rival with the ball, even through other players, and you run 15% faster while moving toward them (within a 45° cone; no bonus while you have the ball). Telegraph (0.5 s, rivals can see it): you crouch low and sniff, and your eyes light up icy blue. Animation: glowing paw prints lead from your feet to the ball carrier, and a translucent wolf head floats above your shoulders, always watching the ball.
- **Awakening (G, 100 aura):** Howl, 5 s: rivals within 7 m when it fires are startled: 30% slower for 1.5 s, with no root and no ball loss. For the whole 5 s, you and teammates within 7 m run 15% faster while moving toward the ball (20% for you in modes without teammates). Telegraph (1 s, rivals can see it): a huge moon rises behind you and you throw your head back to howl. Animation: a sound wave bursts from your mouth, blue rings roll across the ground to the 7 m edge, a pack of three ghost wolves circles you and then runs ahead toward the ball, and exclamation marks flash over startled rivals.
- **In each mode:** Team matches: as written; it sets up a team press. Hot Potato: no teammates. Scent tracks the potato holder so you can keep away from them (the +15% applies while moving away from the holder), and the startle is 20% within 4 m, so a Howl followed by a kick isn't a free elimination. 1v1v1v1: the startle hits every rival within 7 m, the AI keeper is never slowed, and the boost is +20% for you. Final: the solo version (+20%). Volleyball: the boost applies toward the gold landing ring, and the startle doesn't cross the net. Hockey: startled rivals keep sliding on the ice. Basketball and Bomb: as written. Crates: the startle follows the crowd-control rules (2 s immunity afterwards).
- **Counterplay:** When the moon rises, leave the 7 m circle, or pass early: Scent and the pack boost only chase the ball. The startle is short, causes no ball loss and gives 2 s of immunity.
- **Balance:** Conditional speed (a direction cone, no ball) is about value 1.0. The Awakening goes from a mass root to a light slow plus a team buff, which fits a Common and the crowd-control rules.
- **What changed:** Awakening: 'rivals stand still for 1 s within about 8 m' → 30% slower for 1.5 s within 7 m. A root on the cheapest tier was the strongest crowd control in the game and a free elimination in Hot Potato. Team bonus 20% → 15%, with a solo fallback of +20%. Defined the 45° cone. Cooldown 14 s kept. Name kept (Wilk → Wolf).

<details><summary>Awakening animation brief</summary>

Existing 'wolf' fx, Common budget. TELEGRAPH 0–1 s: the scene dims toward navy (tint sphere at 35%). The existing moon rises fast along an arc (scale 0.4 → 1) with a lens-flare sprite and a pale halo while two flat cloud planes slide apart. The figure crouches, then throws its head back at 0.6 s with a puff of breath vapour, and the eyes intensify. The camera drops to 0.8 m and looks up at the player against the moon. At 0.85 s all particles freeze for a beat of silence. BURST at 1.0 s: the howl, with 0.05 s hit-stop. Three flattened sound-wave tori leave the mouth while a hemispherical wireframe wavefront scales 0 → 7 m (opacity 0.4 → 0) and the existing blue rings roll exactly to 7 m, the real radius. FOV punch −4° and a 0.1 s shake. Rival dummies inside get a frost-blue outline, the existing '!' and a 0.2 s hop. LOOP: the three ghost wolves circle once at 2 m, then run in a V formation toward the ball and loop back, leaving fading paw decals and blue streaks. The dashed 7 m edge stays for 1 s, then shrinks into a dotted 'pack' circle around boosted teammates. The player's eyes leave glow trails on turns, and the moon slowly sinks as the countdown. OUTRO (last 0.5 s): the wolves peel off one by one, leap toward the moon and burst into snow-blue mist; the moon fades, the tint lifts and the eyes dim last. Palette #7FA8C9, #DDF3FF, eyes #9BE7FF, moon #EAF6FF. Budget about 90 meshes.

</details>

<a id="ballhawk"></a>
#### Ballhawk  ·  Common  ·  interception

| Cooldown | Ability lasts | Awakening lasts | Chance per roll | Colours | 3D effect | Formerly |
|---|---|---|---|---|---|---|
| 13 s | 3 s | 5 s | 7.43% | `#C8742E` `#FFE0A8` | `hawk` (to build) | new style |

- **Ability (1):** Keen Eye: for 3 s your interception reach is 1 m wider: you stretch out a leg and the ball deflects into your path. It doesn't stick to your foot, so receiving stays the same for everyone. You also see the flight line of every rival pass and lob the moment it's struck. Telegraph (0.5 s, rivals can see it): you shade your eyes with one hand and an amber feather spins above your head. Animation: an amber scan ring sweeps out from you, an amber ring on the ground marks your reach, rival passes leave thin amber lines on the grass, and every interception snaps with a small burst of feathers.
- **Awakening (G, 100 aura):** Talon, 5 s: your interception reach is 2 m wider, and after each interception you run 25% faster for 1.5 s. Telegraph (1 s, rivals can see it): the shadow of huge wings sweeps over the ground around you, you crouch with your arms spread, and your reach ring slams onto the ground. Animation: ghostly amber wings open on your back and beat slowly; every interception plays a short lunge streak, the wings snap forward, feathers burst with a sharp screech, and an amber trail follows you on the counter-attack.
- **In each mode:** Team matches and Bomb: as written; strong at winning the ball back. Volleyball: slide-saves reach 1 m (Keen Eye) or 2 m (Talon) farther. Hot Potato: the reach never makes you the holder. Keen Eye shows the potato's flight line the moment it's kicked, so you can sidestep; during Talon the first potato kicked at you within reach is deflected away and the kicker stays the holder (once per Awakening). 1v1v1v1 and Final (no passes): the reach works on loose balls and on shots that fly past within range, so it acts as a shot block; the AI keeper is unaffected. Basketball: works on passes as written. Hockey: deflections slide farther on ice.
- **Counterplay:** The amber ring shows exactly how far the reach goes: pass around it, chip over it, dribble instead, or hold the pass for 3 s. During Talon the wings and the ring are visible from across the pitch, so switch play to the other side.
- **Balance:** Fills the missing defender who reads passes instead of sliding; the only Common defender was Wolf. Interceptions only happen when the rival chooses to pass near you, and the deflection (not a clean catch) keeps reception identical. +2 m is the largest reach in the game, but it is passive: no automatic dive or steal. The speed after an interception is short and inside the +35% cap.
- **What changed:** New original style, taken from the economy and spectacle proposals. It makes Common the largest tier, gives every role both an attacker and a defender, and counters the passing styles (Playmaker, Illusionist). The spectacle proposal's automatic 'Swoop' steal was not used, because automatic effects are banned. Needs a new fx.

<details><summary>Awakening animation brief</summary>

New fx 'hawk', Common budget. BUILD: two wings, each 6 feather planes (tapered thin boxes or ShapeGeometry) fanned on a pivot group; a pool of 30 feather planes that see-saw as they fall (a sine wobble on rotation.z); a dashed reach ring (RingGeometry with a dashed CanvasTexture) at 3.5 m; a wing-shadow plane (dark, 25% opacity, normal blending); a rival passer and a receiver dummy. TELEGRAPH 0–1 s: 0–0.5 s the wing shadow sweeps once around the player over the ground; 0.5–1.0 s the arms spread while amber feathers are pulled in toward the back; the camera rises 1 m to show the zone; a short screech at 0.9 s. BURST at 1.0 s: the wings snap open (scale 0 → 1 with ease-out-back over 0.3 s), the reach ring slams down (scale 0 → 1.1 → 1), 20 feathers burst outward, 0.05 s hit-stop and an FOV punch of −4°. LOOP: the wings beat at 0.6 Hz and the ring turns slowly with tick marks. Every 2 s the rival dummy passes across the ring: as the ball crosses it, a 0.06 s hit-stop, the body stretches into a 0.3 s lunge streak (scale.z 1.8), the ball deflects onto the boot with an amber flash and ring, the wings snap forward and feathers explode; then a 1.5 s amber speed ribbon shows the counter-attack. An amber countdown ring drains. OUTRO (last 0.5 s): the wings fold and break into feathers that see-saw down and fade on the grass, and the ring fades. Palette #C8742E, #FFE0A8, shadow #2A1A0E. Budget about 90 meshes.

</details>

### Rare styles

<a id="groove"></a>
#### Groove  ·  Rare  ·  dribbling

| Cooldown | Ability lasts | Awakening lasts | Chance per roll | Colours | 3D effect | Formerly |
|---|---|---|---|---|---|---|
| 17 s | 5 s | 6 s | 5.6% | `#FFC233` `#FF7AC6` | `dance` | Taniec |

- **Ability (1):** Footwork: for 5 s your stepover and rainbow-flick cooldowns are 40% shorter (stepover 2 s → 1.2 s, rainbow 4 s → 2.4 s). Stamina costs are unchanged. Telegraph (0.5 s, rivals can see it): the ball starts bouncing on your foot and the first music notes appear. Animation: gold and pink notes swirl around your feet, and glowing footprints light up under you in time with each dribble.
- **Awakening (G, 100 aura):** Showtime, 6 s: stepover and rainbow cooldowns are 60% shorter (0.8 s and 1.6 s) and dribbles cost no stamina. Every dribble that beats a slide tackle makes you 15% faster for 1 s (no stacking). There is no automatic dodge: you still have to time every dribble, and dribbles during Showtime give no aura. Telegraph (1 s, rivals can see it): a pirouette with a flick of the ball, and a glowing dance floor spreads from your feet. Animation: floor tiles flash to the beat, a disco ball with coloured beams hangs above you, the ball circles around you, two hologram dancers copy your moves and notes float upward. Every slide you beat triggers a pirouette with a burst of notes.
- **In each mode:** Team matches, Basketball and Bomb: as written. 1v1v1v1 and Final: as written; dribbling is the main tool here. Hot Potato: Footwork and Showtime shorten the stepover you can do while holding the potato, a juke to fake out the rival you're aiming at; it never gives immunity. Volleyball: no dribbling, so Footwork cuts your slide-save cooldown by 40% and Showtime by 60%. Hockey: dribbles on ice work as normal. Crates: no interaction.
- **Counterplay:** Don't slide into it: jockey, block the path and intercept the next pass, because dribbles only beat slide tackles. Only the first 0.35 s of each dribble beats a slide (global rule), so a slide timed just after a dribble still wins. The dance floor announces 6 s of Showtime, so drop off and wait it out.
- **Balance:** Even with 0.8 s cooldowns, only 0.35 s of each cycle beats a slide (about 44% coverage), so timing still decides. The old version (no cooldown, a free auto-dodge, +10 aura per beaten slide) made you untackleable for 6 s and recharged itself. Value about 1.3 per use on 17 s.
- **What changed:** Ability: −50% for 5 s → −40% for 5 s. Awakening: 'no cooldown, no stamina cost, automatic pirouette on the first slide' → −60% cooldowns, free stamina and a speed reward for beating slides, with no automatic dodge and no aura farming. Added Hot Potato and volleyball versions. Cooldown 18 → 17 s. Renamed Taniec → Groove.

<details><summary>Awakening animation brief</summary>

Existing 'dance' fx, Rare budget (area up to 7 m, 25% flash, name banner, 0.06 s hit-stop, 6° punch). TELEGRAPH 0–1 s on a 4-beat count at 120 BPM (0.25 s per beat): each beat reveals one more ring of floor tiles from the centre outward (the existing radial reveal, each tile with a 1-frame white flash), with a kick-drum camera dip (−0.03) and a burst of notes. The pirouette lands on beat 3 and the ball flick on beat 4. BURST at 1.0 s: the disco ball drops on its cord from 8 m to 3.7 m with ease-out-bounce, with 0.06 s hit-stop as it locks. All six beams switch on at once, every tile flashes white for 2 frames, 30 confetti notes pop, a 25% white screen flash and an FOV punch of −6°, and a 'SHOWTIME' banner slides in. LOOP: tiles pulse in a 4-beat pattern (floor(t*8)%4) while the beams sweep. The hologram dancers mirror every move 0.15 s late instead of standing still. Every 1.5 s a slider dummy comes in and the player stepovers past it; that beaten slide is the hero moment: a 0.05 s freeze pose, the pirouette with a ribbon trail, a pink-and-gold star ring, a beat-drop beam strobe, the slider passing in 0.3 s of 30% slow motion (on the viewer's camera only) and a 1 s speed trail. A countdown ring drains around the floor. OUTRO (last 4 beats): the beams shut off one per beat, the tiles switch off from the outside in, the disco ball winches up out of view, the player strikes a freeze pose, and the notes float up and pop. Palette #FFC233, #FF7AC6, disco accents #2EE6FF and #B26BFF. Budget about 120 meshes.

</details>

<a id="acrobat"></a>
#### Acrobat  ·  Rare  ·  volleys and bicycle kicks

| Cooldown | Ability lasts | Awakening lasts | Chance per roll | Colours | 3D effect | Formerly |
|---|---|---|---|---|---|---|
| 16 s | 4 s | 5 s | 5.6% | `#FF4FD8` `#FFE066` | `acro` | Akrobata |

- **Ability (1):** Spring: for 4 s you jump 35% higher and recover from landings 30% faster. As always, you can't jump with the ball. Telegraph (0.5 s, rivals can see it): you clap your hands and a puff of white chalk rises, like a gymnast. Animation: you run normally; only when you jump does a small trampoline pop up under your feet for a split second, flex, launch you and vanish. Chalk sprays at take-off, a pink ribbon trails from your hands in the air, and every second jump is a somersault.
- **Awakening (G, 100 aura):** Big Top, 5 s: you jump 35% higher, and your volleys and bicycle kicks are 25% stronger (within the 130% shot cap) with a 50% longer hit window, so they are easier to time. Telegraph (1 s, rivals can see it): a spotlight snaps on from above and locks onto you, and a red-and-white circus ring rises around your feet. Animation: the spotlight follows you for the whole Awakening, and a flaming hoop hovers in front of you at shooting height. When you hit a volley or bicycle kick, the full act fires: ribbons stream from both feet, the ball flies through the flaming hoop, the hoop bursts into fire and confetti, and you land with a bow in a cloud of chalk.
- **In each mode:** Team matches, Hockey and Bomb: as written. Basketball: points come from jumps, so both jump bonuses are +20%. Volleyball: jump bonuses are +20%, and strikes in the air count as volleys. Hot Potato: Spring is +25% (jumping over a low potato is a dodge), and volley kicks at players are capped at +15%. 1v1v1v1 and Final: volleys come from rebounds and lobs, so it is situational; keepers save normally. Crates: the super-jump crate doesn't stack (the higher value applies, cap +50%).
- **Counterplay:** Volleys need a bouncing or crossed ball: keep it on the ground near the Acrobat and don't clear it high. The spotlight shows Big Top from across the pitch, and landing is still a vulnerable moment.
- **Balance:** Height and volley power only matter around aerial balls, so per-use value is about 1.3. The longer hit window lowers the skill floor of the hardest shot in the game without raising its ceiling past the 130% cap. Acrobat owns volleys and bicycle kicks, Tower owns headers and Hourglass owns time, so the three no longer overlap.
- **What changed:** 'Much higher' → +35% jump, with per-mode caps. 'Explosive volleys from much farther' → +25% power and a 50% longer hit window. Awakening 6 s → 5 s. Added a hovering hoop so the Awakening reads before any volley happens. Cooldown 18 → 16 s. New magenta colour, so it no longer shares Groove's gold-and-pink pair. Renamed Akrobata → Acrobat.

<details><summary>Awakening animation brief</summary>

Existing 'acro' fx, Rare budget. TELEGRAPH 0–1 s: a drumroll and the arena dims 25%. 0–0.3 s the spotlight cone sweeps in from off-stage and narrows onto the player in 3 beats (narrow, wider, full). 0.3–0.7 s the red-and-white circus curb pops up segment by segment (12 box segments, ease-out-back). 0.7–1.0 s bunting garlands snap tight overhead, a striped canopy of 8 triangular panels (magenta and yellow, crown only, radius 3 m) unfurls from a central pole, and a chalk clap puffs. BURST at 1.0 s: the spotlight locks on with a white flash, two confetti cannons fire 50 pieces from the curb, a 25% pink screen flash, an FOV punch of −6° and a 'BIG TOP' banner. LOOP: the spotlight follows with 0.1 s lag. A flaming hoop hovers in front of the player at shooting height and turns to face the camera, so the Awakening reads even with no volley. Every jump shows the trampoline flash, a chalk puff and hand ribbons. Every 2 s a ball is lobbed in and the player does a bicycle kick: 0.07 s hit-stop on the contact frame while upside down, ribbons from both feet; the hoop moves onto the ball's real flight path 4 m out and turns to face along it; the ball passes through and the hoop bursts into 14 flame cones plus confetti, with a soft crowd 'ooh'. A countdown ring drains. OUTRO (last 0.6 s): a bow in a chalk cloud, the canopy panels fold into the pole, the curb segments sink, the hoop goes out in a puff of smoke, and the spotlight irises closed (cone radius → 0). Palette #FF4FD8, #FFE066, curb #E53935 and white, hoop fire orange. Budget about 130 meshes.

</details>

<a id="glacier"></a>
#### Glacier  ·  Rare  ·  ice and slide

| Cooldown | Ability lasts | Awakening lasts | Chance per roll | Colours | 3D effect | Formerly |
|---|---|---|---|---|---|---|
| 17 s | 4 s | 5 s | 5.6% | `#4FC3FF` `#E6FAFF` | `ice` | Lodowiec |

- **Ability (1):** Frost Trail: your next slide tackle within 4 s leaves an ice strip 6 m long and 1.5 m wide that melts after 3 s. Everyone on it except you (teammates too) gets ice footing: 50% less acceleration and turning. Players in the air aren't affected. Telegraph (0.5 s, rivals can see it): a puff of breath vapour leaves your mouth and frost covers your boots. Animation: the slide leaves a glittering ice strip with crystals along its edges, snowflakes spray from your boots, and anyone who runs onto the ice spins and slides sideways.
- **Awakening (G, 100 aura):** Whiteout, 5 s: a ring of ice with a 5 m radius follows you. Rivals inside get ice footing (40% less acceleration and turning), and sprinting there costs double stamina; you skate 15% faster. The ball isn't affected. Telegraph (1 s, rivals can see it): snow starts swirling around you and the first ice crystals push out of the ground at the ring's edge. Animation: a disc of ice with a cracking pattern spreads under you, crystals stand along its edge, a blizzard swirls inside it and skate blades glow under your boots. You glide like a figure skater while rivals in the ring wobble and lose their footing.
- **In each mode:** Team matches, Bomb and 1v1v1v1: as written; the strip also catches careless teammates, and the AI keeper ignores ice. Final: as written (5 m is already the radius cap); in the volcano the ice hisses with steam (visual only). Hot Potato: the strip is 4 m long and the ring 4 m. Basketball and volleyball: as written (the sand freezes too); in volleyball the strip only forms on your side of the net. Hockey: everything is already ice, so Frost Trail gives you normal grip for 3 s instead, and Whiteout gives you grip plus 15% speed for 5 s, with no ring. Crates: doesn't stack with the freeze crate.
- **Counterplay:** Walk around the strip or jump over it; it's bright and lasts only 3 s. The ring's edge is clearly drawn and centred on the Glacier, so stay more than 5 m away, or pass the ball out of the ring: passes and shots work normally inside it.
- **Balance:** A zone debuff with a friendly-fire cost on the strip, value about 1.3. The Awakening radius dropped from about 10 m (a third of a street pitch) to 5 m, and the sprint ban became a stamina cost, so rivals can always escape.
- **What changed:** Ability: added the 4 s window, the strip width and exact ice numbers; the strip now affects teammates too. Awakening: ring of about 10 m → 5 m, 'can't sprint' → double sprint cost, own speed +20% → +15%. Added the Hockey grip version, because the original did nothing on ice. Cooldown 19 → 17 s. Renamed Lodowiec → Glacier, and the Awakening Zamieć → Whiteout.

<details><summary>Awakening animation brief</summary>

Existing 'ice' fx, Rare budget. TELEGRAPH 0–1 s: breath puffs at 4 Hz. 0–0.4 s snow sprites start orbiting wide and tighten toward the player (the existing blizzard). 0.4–0.8 s frost creeps outward as a growing disc with a crackle CanvasTexture, and 6 first crystals poke out exactly at 5 m, the true radius. 0.8–1.0 s the air goes still and a light-blue overlay creeps in. BURST at 1.0 s: 0.06 s hit-stop, a 30% white-blue flash and an FOV punch of −6°. The ice disc snaps to 5 m in 0.25 s with crack lines drawn from the centre; 20 edge crystals spring up in order around the circle, 0.02 s apart, with a glass chime and 16 ice shards flying up; a 'WHITEOUT' banner. LOOP: 60 snow sprites swirl inside the ring, faster near the edge so the boundary reads. A low mist wall (open CylinderGeometry, opacity 0.15) marks the edge. The skate blades glow and the skating figure carves twin white scratch trails (a ribbon). Rival dummies that enter get a frosted tint, a stamina-drop icon and a wobble. An ice-blue countdown ring drains along the edge. OUTRO (last 0.5 s): cracks race to the edge, the disc breaks into 30 plate shards that pop up 0.3 m and fade, the crystals shatter outward into snow, a dark wet disc (the melt) fades over 0.5 s, and the blades melt into drips. Camera: a slight top-down lift during the loop to show the ring. Palette #4FC3FF, #E6FAFF, crack lines #1F5FA8. Budget about 110 meshes.

</details>

<a id="ember"></a>
#### Ember  ·  Rare  ·  stamina and fire

| Cooldown | Ability lasts | Awakening lasts | Chance per roll | Colours | 3D effect | Formerly |
|---|---|---|---|---|---|---|
| 18 s | 3 s | 5 s | 5.6% | `#FF5A1F` `#FFD23F` | `phoenix` | Feniks |

- **Ability (1):** Second Wind: you instantly get 35 stamina back, and for 3 s your stamina keeps refilling at the normal resting rate even while you sprint. Telegraph (0.5 s, rivals can see it): you clench your fists, and ash and the first sparks rise from under your feet. Animation: burning wings unfold on your back and throw out a cloud of sparks with one beat, and the stamina bar above your head fills with a wave of fire.
- **Awakening (G, 100 aura):** Rekindle, 5 s: your stamina refills to 100, and sprints, slides and dribbles cost nothing. The first time a rival tackles the ball off you in these 5 s, a ring of flame bursts out: the tackler is pushed back 2 m and staggered for 0.4 s, and the ball pops up loose 1 m in front of where you stood. It doesn't return to you: anyone can take it, and the tackler still gets the tackle's aura and points. Telegraph (1 s, rivals can see it): you crumble into a pile of glowing ash and a column of fire bursts out of it. Animation: you rise from the fire with huge burning wings and a tail of feathers, burning feathers drift around you, a firebird circles overhead, and a glowing ring on your chest shows the flame ring is armed. When a rival tackles you, the ring explodes, the rival is thrown back and the ball pops up in a burst of sparks.
- **In each mode:** Team matches, Basketball and Bomb: as written; free stamina keeps you pressing. 1v1v1v1 and Final: as written; the loose ball is a real 50/50 (you are about 0.3 s closer, and the tackler is staggered). Hot Potato: reversed, because getting the potato back would blow you up: the first potato that hits you during Rekindle is knocked back toward the kicker by the flame ring, and the kicker stays the holder (once per Awakening; the last-10-s lock applies). Volleyball: there are no tackles, so the ring triggers once on the first ball about to land within 3 m of you and pops it up. Hockey: the loose ball slides farther. Crates: the stagger follows the crowd-control rules.
- **Counterplay:** Bait the ring: the chest ring shows it's armed, so make a cheap first tackle with a teammate nearby to collect the loose ball, or intercept passes instead (the ring only triggers on tackles). The ash pile is an obvious 1 s warning, and draining stamina before it fires still works.
- **Balance:** 35 stamina every 18 s is about one extra sprint (value about 1.3) and keeps stamina management relevant. The Awakening no longer cancels a correct tackle; it only turns it into a contested ball, and the defender keeps the reward.
- **What changed:** Epic → Rare: a stamina tool with one insurance. Instant stamina 50 → 35; cooldown 24 → 18 s. Awakening 6 s → 5 s; the ball no longer returns to your foot, it pops up loose. Added Hot Potato and volleyball versions; the original would have handed the potato back to you. Renamed Feniks → Ember and Odrodzenie → Rekindle, because a fire hero with a rebirth ultimate is too close to a famous shooter character.

<details><summary>Awakening animation brief</summary>

Existing 'phoenix' fx, Rare budget. TELEGRAPH 0–1 s: instead of squashing the figure, break it into 40 small ember cubes that tumble into the ash cone (0–0.4 s). Embers glow through cracks and heat haze rises (0.4–0.8 s). The pile pulses three times, brighter each time, with a low rumble. BURST at 1.0 s: the fire pillar erupts 0 → 6 m in 0.15 s with a whoosh, with 0.07 s hit-stop at its peak, a 35% orange flash, an FOV punch of −6° and the camera tilting up 10° with the pillar. 70 sparks and 18 feathers burst out. The player rises out of the flames as the wings snap open (scale 0 → 1.15 → 1 over 0.3 s; each wing 7 feather quads, twice today's size and fully opaque at the core, so they read from the showcase camera). One huge flap sends a fire ring across the ground, the stamina-bar hologram fills with a fiery sweep, and a 'REKINDLE' banner appears. LOOP: the wings beat at 1.5 Hz, shedding burning feathers; a flame tail sways; the firebird orbits at 3.2 m dragging a fire ribbon; a glowing chest ring pulses (armed); ash footprints. At 2.5 s a tackler dummy slides in: the chest ring breaks, a 2 m fire ring explodes with 0.05 s hit-stop, the tackler tumbles back trailing embers, and the ball pops up 1 m in front with a spark comet, not back to the player. An orange countdown ring drains. OUTRO (last 0.6 s): the wings fold and burn away from the tips into embers drifting upward, the firebird dives into the player as a flash, the body colour cools and a small smoke puff rises. Palette #FF5A1F, #FFD23F, white-hot #FFF2B0, ash #3A3438. Budget about 120 meshes.

</details>

<a id="snapshot"></a>
#### Snapshot  ·  Rare  ·  quick shot

| Cooldown | Ability lasts | Awakening lasts | Chance per roll | Colours | 3D effect | Formerly |
|---|---|---|---|---|---|---|
| 16 s | 4 s | 5 s | 5.6% | `#FFF3C4` `#FFD23F` | `impact` | Impakt |

- **Ability (1):** Quick Load: for 4 s your shots and lobs charge twice as fast (full power in 0.5 s instead of 1 s). Power, speed and accuracy are unchanged. Telegraph (0.5 s, rivals can see it): your kicking leg pulls back and the air around the boot shimmers like heat over asphalt. Animation: gold energy rings tighten around your leg like a spring being wound, rings of shimmering air spread from the boot, and a hologram power bar beside you fills in a blink.
- **Awakening (G, 100 aura):** Sound Barrier, 5 s: every shot and lob you take is full power the instant you press (a tap is a full charge), and curve still works. The first shot is also 10% faster. Telegraph (1 s, rivals can see it): the world breathes in: a big bubble of energy and dust from all around are sucked into your boot. Animation: the kick lands with a comic-panel flash and radial speed lines, the ball breaks the sound barrier inside a white vapour cone with shock rings bursting behind it, a cracked crater is left under your foot and the camera shakes.
- **In each mode:** Team matches, Hockey and Bomb: as written. Hot Potato: charging is 1.5× faster instead of 2× because aim assist already helps kicks at players, and Sound Barrier's first kick gets +5%. 1v1v1v1 and Final: the AI keeper keeps its normal reaction time, so the surprise is when you shoot, not an unsavable ball; it works best right after a rival commits to a slide. Basketball: instant power can overshoot the hoop, so it's a sidegrade there. Volleyball: instant full-power strikes on the return. Crates: the power-shot crate doesn't stack with the +10%.
- **Counterplay:** The breathing-in telegraph warns that shots are coming: close the shooting lane before the 1 s ends. Shots reach only normal full power (+10% on the first), so a set keeper or a blocker still stops them.
- **Balance:** Tempo, not power: removing the wind-up takes time away from defenders. A whole 4 s window instead of a single shot lifts it to Rare value 1.3 on 16 s. The Awakening is 5 s of time denial, not a stronger ball, so it fits the shared Awakening budget.
- **What changed:** Epic → Rare: one simple effect with a timing twist. The 'next shot' buff became a 4 s window covering every shot in it. Awakening: one instant shot played as a 6 s effect (100 aura only saved 1 s of charging) → a clear 5 s window of instant full power plus +10% on the first shot. Cooldown 22 → 16 s. Renamed Impakt → Snapshot, a real football term for a quick shot, which also keeps it apart from Cannon (how fast vs how hard). New white-gold colour, so it no longer shares Tempest's yellow.

<details><summary>Awakening animation brief</summary>

Existing 'impact' fx, Rare budget. TELEGRAPH 0–1 s in three beats. 0–0.4 s dust motes drift in and the inhale sphere starts shrinking from 3 m with ease-in. 0.4–0.8 s twelve turf chips and 40 motes stream along curved paths into the boot, the point light dims 30% (a vacuum), the background desaturates (tint plane) with a rising rumble, and the leg coils tighten (scale 1 → 0.6) and turn white. 0.8–0.95 s total stillness: every particle freezes, the leg is pulled far back (rotation 1.4) and the sound cuts out. BURST at 1.0 s: a comic-panel freeze. 0.08 s hit-stop with the figure flashed white and the radial speed lines (a camera-facing plane with a CanvasTexture of ink lines) at full opacity for 2 frames; a 6 m white flash sprite and a 25% screen flash; an FOV punch 36 → 30 and back over 0.25 s. The ball leaves in the white vapour cone with 4 shock diamonds (thin cone rings spaced along the path) and a shock ring every 0.06 s. The crater decal and 8 cracks grow to 1.2 m in 0.15 s, 12 turf chunks fly up under gravity, a 0.25 s shake at 0.08, and a 'SOUND BARRIER' banner. LOOP: 1.6–2.1 s the first strike replays from a side camera at 30% speed (re-run the shot timeline with dt × 0.3), a built-in clip replay. The boot stays gold-lit and a heat-shimmer ring pulses every 0.5 s (instant charge is live). Taps at 2.6 s and 3.8 s repeat a smaller flash with fewer rings and no wind-up. The crater glows amber and slowly cools, and a gold countdown ring drains. OUTRO (last 0.6 s): the coils unwind in a spark spray, vapour rings drift outward and fade, and the crater cools to dark and fades over 1 s. Palette white-hot #FFF3C4, gold #FFD23F, black ink lines. Budget about 90 meshes.

</details>

### Epic styles

<a id="monarch"></a>
#### Monarch  ·  Epic  ·  strength

| Cooldown | Ability lasts | Awakening lasts | Chance per roll | Colours | 3D effect | Formerly |
|---|---|---|---|---|---|---|
| 21 s | 2.5 s | 5 s | 3.5% | `#B26BFF` `#FFC94A` | `king` | Król |

- **Ability (1):** Crest Shield: for 2.5 s, slide tackles that hit you from the front (a 90° cone) don't take the ball, and the slider bounces back 1.5 m. Tackles from the side or behind work as normal, and you move 10% slower while the shield is up. Telegraph (0.5 s, rivals can see it): you square your shoulders and a purple ring flashes under your feet. Animation: a knight's shield appears in front of you, purple with a gold rim, a cross and a crown; a rival who slides into it from the front bounces off in sparks.
- **Awakening (G, 100 aura):** Coronation, 5 s: while you have the ball, rivals who come within 1.5 m are shoved back 2.5 m and staggered for 0.3 s (each rival only once). Your next shot in these 5 s is 30% faster, and a body block deflects it (the ball keeps 40% of its speed) instead of stopping it dead. Goalkeepers are never shoved and save it as normal. Telegraph (1 s, rivals can see it): a golden crown descends onto your head and a royal crest blooms on the ground under your feet. Animation: the crown shines, a purple cape flares out and a purple-and-gold aura burns around you; shoved rivals tumble backward with a shockwave and stars, and the shot flies wrapped in gold-and-purple fire, glancing off any blocker in a burst of sparks.
- **In each mode:** Team matches and Bomb: as written. Hot Potato: Crest Shield makes the first potato that hits you from the front bounce off loose, and the kicker stays the holder; Coronation's shove doesn't move the potato, and its kick gets +15%. 1v1v1v1: as written; the AI keeper is never shoved. Final: the shove works once on the only rival, so it buys one step, not a walk-in. Basketball: the +30% applies to ground shots only. Volleyball: no contact across the net, so Coronation gives +30% strike speed only. Hockey: shoved rivals slide 3 m on ice.
- **Counterplay:** During Crest Shield, tackle from the side or from behind, and use its 10% slowness to get around. Coronation shoves each rival only once, so send two defenders. The shot can be blocked or deflected and keepers save it normally, and the crown gives 1 s to set up.
- **Balance:** A conditional guard with a speed tax, value about 1.6. The Awakening drops the unblockable 2× shot and the shove-everyone aura: one strong chance instead of three stacked effects, and nothing gets past a keeper.
- **What changed:** Ability: 3 s → 2.5 s, with a defined 90° cone and a 10% slow. Awakening: a 2× shot that passes through blockers → +30% and deflectable; the shove now hits each rival once, only while you have the ball, and never keepers. Cooldown 24 → 21 s. Renamed Król → Monarch, because 'King' is the nickname of a famous anime striker with the same physical fantasy; the ability is named Crest Shield to avoid a famous action-game style.

<details><summary>Awakening animation brief</summary>

Existing 'king' fx, Epic budget (summoned set piece, 40% flash, 0.08 s hit-stop, 8° punch, music sting). TELEGRAPH 0–1 s: a gold light shaft (additive cylinder) lands on the player. The crown descends from 5 m, spinning slowly (2 turns) with a halo ring and shedding gold glitter, and lands exactly at 1.0 s. The crest blooms on the ground (8 petal planes opening clockwise), a purple carpet with gold edges unrolls 4 m forward (scale.z 0 → 1), and two crest banners rise on either side. The camera drops to a low heroic angle and a brass fanfare swells. BURST at 1.0 s: the crown lands with 0.08 s hit-stop, a gold flash sphere (0 → 1.5 m), a 40% gold screen flash and an FOV punch of −8°; the camera dips 0.1 m like a bow. A purple shockwave ring expands exactly to 1.5 m (the shove radius), 30 stars burst, the purple cape (a plane with a sine-wave vertex wobble) flares out, and a 'CORONATION' banner appears. LOOP: twin flame cones (purple inside, gold outside) flicker, and a faint purple dome marks the 1.5 m bubble. Two rival dummies approach one after the other: each gets a gong hit, 0.04 s hit-stop and a white wave ring, and tumbles 2.5 m with 6 stars. At 3.2 s the empowered shot flies with 10 alternating purple and gold trail spheres plus fire, hits a blocker dummy and glances off at 30° with a cone of gold sparks (a deflection, never a pass-through). A countdown ring drains on the crest. OUTRO (last 0.6 s): the crown lifts off, spins up and dissolves into gold dust, the carpet rolls back, the banners sink, and the crest fades petal by petal in reverse. Palette #B26BFF, #FFC94A, deep #5A2A9A, gem #E0245E. Budget about 100 meshes.

</details>

<a id="bulwark"></a>
#### Bulwark  ·  Epic  ·  defense

| Cooldown | Ability lasts | Awakening lasts | Chance per roll | Colours | 3D effect | Formerly |
|---|---|---|---|---|---|---|
| 21 s | 3 s | 4 s | 3.5% | `#6FAE4E` `#D8F5C0` | `wall` | Ściana |

- **Ability (1):** Stone Guard: for 3 s your slide-tackle and block reach is 40% longer (1.5 m → 2.1 m), and you get up 50% faster after a missed slide. Telegraph (0.5 s, rivals can see it): you stamp, the ground cracks and dust rises. Animation: stone guards grow on your forearms and shins, rock shards circle your waist, and a ring of stones on the ground marks how far you can reach.
- **Awakening (G, 100 aura):** Fortress, 4 s: a stone wall 3 m wide and 2.2 m tall rises 1.5 m behind you and follows you with a 0.3 s lag; you move 15% slower while it stands. Shots and passes that hit it bounce off, players walk around it, and lobs fly over it. It has 3 HP: a normal shot takes 1 and a full-power or stronger shot takes 2, and at 0 it crumbles. It can't follow you within 8 m of your own goal line; there it stops on the 8 m line. Blocks by the wall give no aura. Telegraph (1 s, rivals can see it): the ground shakes, cracks spread from your feet and the first stones push out of the ground. Animation: the wall grows out of the ground row by row, with battlements on top, a green banner in the middle, torches at the sides and three gems on the banner showing its HP. A ball that hits it bounces off in a cloud of dust, a crack appears and a gem goes dark.
- **In each mode:** Team matches, Hockey and Final: as written. Bomb: as written; the 8 m rule matters most here, so the bomb team can always shoot from close range. 1v1v1v1: the 8 m rule uses the shared goal, so nobody can wall it off, and the wall has 2 HP. Basketball: the wall can't come within 8 m of either hoop. Volleyball: the wall can't rise; Fortress instead gives you 2 m reach on strikes and slide-saves for 4 s. Hot Potato: Stone Guard lets you reach the potato from 2.1 m when you're the holder, so you can kick it on sooner; Fortress is a 2 m wall that stops one potato and then crumbles (the kicker stays the holder). Crates: no interaction.
- **Counterplay:** Lob over it, curve around it, or break it with two strong shots. It trails behind the Bulwark, so drag them away from the play or shoot from the side. It can never close the goal mouth (the 8 m rule), and the Bulwark is 15% slower while it stands, so dribble past them.
- **Balance:** The original wall moved with you and stopped every shot for 5 s: parked on the goal line it was a shutout that decided Finals and Match 5 on its own. 3 HP (with strong shots counting double), lobs over the top, the 8 m rule, a 15% slow and no aura from blocks make it a breakable, dodgeable obstacle with a value of about 1.6.
- **What changed:** Kept the original fantasy of a wall behind you that moves with you. Ability: 'more reach' → +40%, plus faster recovery after a missed slide. Awakening 5 s → 4 s, with a defined size (3 × 2.2 m), 3 HP, lobs over the top, the 8 m goal rule, the 15% slow and no aura from blocks. Cooldown 22 → 21 s. Renamed Ściana → Bulwark, Awakening Fortress; Rampart and Bastion were avoided because they are famous shooter characters.

<details><summary>Awakening animation brief</summary>

Existing 'wall' fx, Epic budget. TELEGRAPH 0–1 s, an earthquake: three escalating ground shakes (camera 0.03, 0.05, 0.08), floor and figure jitter of 0.02, and 20 pebbles hopping. Crack decals race 1.5 m back to the wall line, a green rune circle lights under the feet, and 4 foundation stones poke up, glowing green along the cracks. At 0.8 s the banner pole rises. BURST at 1.0 s: the wall rows rise in 0.35 s (faster than today) with stone-on-stone thuds and dust jets on both sides; 0.08 s hit-stop when the battlements lock, a 35% green-white flash and an FOV punch of −8°. The banner unfurls (a sine wobble on a plane), the torches ignite in sequence, a 30-puff dust wave rolls outward, and a 'FORTRESS' banner appears. LOOP: torch flicker light plays on the stone, and three HP gems glow on the banner. The wall slides after the player with a 0.3 s lag and a dust trail at its base, and the player's steps look heavier (the 15% slow). A shooter dummy fires at 1.8 s: a normal shot gives a dust burst, flying chips, a crack decal, one gem going dark, 0.05 s hit-stop and a heavy thud. At 3.0 s a full-power shot takes two gems at once. A countdown ring drains at the base. OUTRO: when time runs out or the last gem dies, the wall crumbles from the top down into 30 tumbling bricks with gravity and random spin, row by row 0.05 s apart; the banner falls and fades, the torches tip over and go out, and a final dust plume rises. Palette #6FAE4E, #D8F5C0, stone #8A8577, banner #2E8B45, torch #FFB21A. Budget about 110 meshes.

</details>

<a id="illusionist"></a>
#### Illusionist  ·  Epic  ·  tricks

| Cooldown | Ability lasts | Awakening lasts | Chance per roll | Colours | 3D effect | Formerly |
|---|---|---|---|---|---|---|
| 20 s | 4 s | 5 s | 3.5% | `#E0457B` `#FFD86B` | `magic` | Iluzjonista |

- **Ability (1):** Sleight: your next pass or shot within 4 s bends once around the first rival in its path (up to 1.5 m sideways). It won't bend around a rival standing within 2 m of the target, or ever around a goalkeeper, and the bend costs 15% of the ball's speed. Telegraph (0.5 s, rivals can see it): a playing card appears in your hand and you twirl it in your fingers. Animation: the ball leaves gold dust and little stars behind it, and when a rival reaches for it, it neatly curves around them and a puff of cards bursts where they tried to catch it.
- **Awakening (G, 100 aura):** Grand Finale, 5 s: your next 2 passes or shots each release a hologram ball that flies at the same speed toward a point up to 25° away from the real one. Holograms cast no shadow, make no kick sound, flicker every 0.3 s and vanish after 1.2 s or when they touch anything. AI goalkeepers always follow the real ball but react 0.15 s later. Telegraph (1 s, rivals can see it): a magician's top hat rises out of the ground, a ball pops out of it and two spotlights snap on. Animation: you wear the top hat, a fan of cards circles you and two card icons over the hat show the holograms left; each kick splits in the air into the real ball and a flickering purple hologram that bursts into cards when it vanishes.
- **In each mode:** Team matches, Hockey and Bomb: as written. Hot Potato: Sleight bends the potato around a rival in the way (aim assist is off for that kick); a hologram can't hit anyone or make anyone the holder, so it's pure bluff. 1v1v1v1 and Final: Sleight works on shots, and the 0.15 s AI keeper delay is the real effect. Basketball: holograms only appear on ground kicks, not on hoop shots. Volleyball: the hologram gets its own flickering landing ring, while the real ring stays solid. Crates: no interaction.
- **Counterplay:** Look at the ground: only the real ball has a shadow and a kick sound, and holograms flicker. There are only 2 per Awakening and the card icons count them down. Mark the receiver instead of the ball. Sleight can't get around a defender standing tight on the receiver, and it is 15% slower.
- **Balance:** Two fakes (not unlimited) with readable tells, value about 1.6. The AI keeper gets a small fixed delay instead of either cheating or being fooled. Sleight now works on shots too, so it isn't dead in the three modes without passing.
- **What changed:** Sleight: added a bend limit, receiver protection, a keeper exception and a 15% speed cost; it now also works on shots. Grand Finale: 6 s of fakes on every kick → 5 s with at most 2 fakes, a shadow tell and a defined AI keeper rule (0.15 s); hologram life 1.5 s → 1.2 s; volleyball rule added. Cooldown 23 → 20 s. Name kept.

<details><summary>Awakening animation brief</summary>

Existing 'magic' fx, Epic budget. TELEGRAPH 0–1 s: 0–0.6 s two red velvet curtain planes (a vertical-stripe CanvasTexture for the folds) slide open behind the player, and the top hat rises out of the ground with a rotating rim glow under a drumroll. 0.6–0.8 s a ball pops out with a spring sound and a puff of cards. At 0.8 s two spotlights swing in and lock on with a clack, and the card fan unfurls 10 cards in sequence. BURST at 1.0 s: the hat jumps onto the head with 0.08 s hit-stop at the top of the ball's arc, a 40% gold flash and an FOV punch of −8°. A ring of 30 cards fans outward, 6 white paper birds (pairs of folded planes, flapping) flutter up, a cymbal ends the drumroll, and a 'GRAND FINALE' banner appears. LOOP: the card fan orbits, with two card icons over the hat brim. At 1.8 s and 3.2 s a kick splits, with a 0.04 s white split flash and a vertical shimmer plane. The real ball casts a crisp shadow disc on the ground; the hologram (translucent purple, scanline flicker at 3.3 Hz, no shadow) flies 25° off. A keeper dummy dives the right way 0.15 s late, the hologram bursts into 8 tumbling cards with a 'ta-da' sparkle, and one card icon flips away. A countdown ring drains. OUTRO (last 0.6 s): the player bows, the hat tips over, rolls and vanishes in a puff of cards, the curtains close and the spotlights iris out. Palette #E0457B, #FFD86B, hologram #C9A8FF, curtains #7A0F2A. Budget about 100 meshes.

</details>

<a id="shade"></a>
#### Shade  ·  Epic  ·  stealth

| Cooldown | Ability lasts | Awakening lasts | Chance per roll | Colours | 3D effect | Formerly |
|---|---|---|---|---|---|---|
| 22 s | 3 s | 4 s | 3.5% | `#5A3DFF` `#C9A8FF` | `ninja` | Cień |

- **Ability (1):** Smoke Bomb: you drop a smoke cloud 4 m wide that lasts 3 s. Players inside it are hidden from rivals outside, who see only a shimmer; rivals within 3 m of you always see you. The ball is never hidden: it glows inside the smoke. Leaving the cloud makes you 10% faster for 1 s. Telegraph (0.5 s, rivals can see it): you reach to your belt for a smoke-bomb pellet. Animation: you throw it at your feet and purple smoke bursts out with falling petals; inside the smoke you leave only fading footprints and a ripple in the air, and you step out of it in a puff.
- **Awakening (G, 100 aura):** Vanishing Cut, 4 s: within the 4 s, press G again to blink up to 7 m in your movement direction, keeping the ball. You can blink past a rival (you always land at least 1 m clear of anyone), but not through walls or goalposts, and not into the goal area. A smoke decoy of you stands at your old spot for 1.5 s, and you can't shoot for 0.5 s after the blink. If you don't blink, 50 aura is refunded. Telegraph (1 s, rivals can see it): a purple ninja seal with spinning runes lights up under your feet, and a faint 7 m ring shows your range. Animation: you melt into smoke, leaving a dark afterimage, a purple streak flashes across the ground, and you reappear past the defender with a crescent-shaped slash while a question mark pops over their head.
- **In each mode:** Team matches, Hockey and Bomb: as written. Hot Potato: the cloud is 3 m wide; aim assist ignores players inside the smoke, but a manual hit still counts, and the potato glows through it. Vanishing Cut is an escape without the potato, or a point-blank delivery with it (the target saw the seal for 1 s). 1v1v1v1: you can't blink into the goal area, and the AI keeper is unaffected. Final: a fixed 7 m with a 0.5 s no-shot lag, so a defender who drops deep still recovers. Basketball and volleyball: the blink is ground-only and stays on your side of the net. Crates: no interaction.
- **Counterplay:** Follow the glowing ball, not the player, and stay within 3 m to keep sight. Against Vanishing Cut, the range ring shows where they can land: drop deeper than 7 m, or use the 0.5 s no-shot lag to recover. The decoy has no ball.
- **Balance:** Information denial instead of true invisibility, value about 1.6. The Awakening is a capped reposition that beats one defender, not a guaranteed open goal: a fixed range, no stun on the defender, a shot lag and no landing in the goal area. The refund stops it from feeling wasted.
- **What changed:** Ability: 2 s of full invisibility (whether the ball vanished too was unclear) → a 3 s smoke cloud with the ball always visible and a 3 m reveal; this also fixes the viewer mismatch (2.6 s vs 2 s). Awakening: 'teleport behind the nearest defender' (no range; it could mean behind the AI keeper) → a player-aimed 7 m blink that can pass a defender, with a decoy, a 0.5 s no-shot lag and no landing in the goal area. Cooldown 25 → 22 s. Renamed Cień → Shade; the Awakening name avoids 'Shadowstep', a famous MMO skill.

<details><summary>Awakening animation brief</summary>

Existing 'ninja' fx, Epic budget. TELEGRAPH 0–1 s: the seal lights in three stages 0.33 s apart (outer ring, star, then 8 runes one by one), each with a metallic 'shing', and the runes spin faster toward 1 s. The light around the player dims 40% except violet, ink-brush strokes (black and violet planes) splash across the ground, and a faint dashed 7 m range ring fades in (gameplay readability). Petals start falling, and the camera pushes in slowly toward the face, where two violet eye glints appear. BURST (the second G press; in the viewer at 1.6 s): 0.1 s hit-stop as the frame goes two-tone with a 40% violet flash. The body dissolves into 20 light, see-through purple puffs and petals (replacing today's dark spheres, which read as blobs on the dark stage). A dark afterimage freezes as the decoy, an ink streak draws across the ground in 0.08 s, and the camera whip-pans to the landing point in 0.15 s. The player appears past the defender dummy; 0.3 s later a wide horizontal violet slash line flashes across the frame, the crescent-slash torus fires, petals burst, and a '?' pops over the dummy. A 'VANISHING CUT' banner. LOOP: before the blink, the seal pulses and the player's outline flickers violet at 4 Hz. After it, a dim boot glow shows the 0.5 s no-shot lag, a smoky afterimage trail follows the player, and the decoy collapses into smoke and petals after 1.5 s. A violet countdown ring drains. OUTRO (last 0.5 s): the smoke disperses, and the seal decal at the starting spot shatters into glyph fragments and fades last, closing the clip. Palette #5A3DFF, #C9A8FF, petals #FFB0E0, ink #0B0614. Budget about 100 meshes.

</details>

### Legendary styles

<a id="tempest"></a>
#### Tempest  ·  Legendary  ·  storm tackling

| Cooldown | Ability lasts | Awakening lasts | Chance per roll | Colours | 3D effect | Formerly |
|---|---|---|---|---|---|---|
| 24 s | 3 s | 5 s | 1.67% | `#FFE14D` `#7FE8FF` | `storm` | Grom |

- **Ability (1):** Lightning Slide: your next slide tackle within 3 s travels 60% farther and 30% faster in a straight line. If it misses, you stay on the ground 0.4 s longer than after a normal missed slide, and a well-timed dribble still beats it. Telegraph (0.5 s, rivals can see it): your hair stands on end and small sparks jump across your body. Animation: the slide turns into a yellow-and-cyan flash, a lightning zigzag is burned into the grass behind you, and a crackling discharge pops where you hit the ball.
- **Awakening (G, 100 aura):** Thunderhead, 5 s: a storm cloud follows you. Every 1.5 s (3 strikes at most) it marks the nearest rival with the ball within 10 m: a 1.2 m yellow ring appears under them for 0.75 s, then lightning hits the ring. If they are still in it with the ball, the ball is knocked 3 m away and they are staggered for 0.3 s. Passing, shooting, a stepover or rainbow at the moment of the strike, or leaving the ring, dodges it. It never targets goalkeepers, and strikes give no aura. Telegraph (1 s, rivals can see it): the sky darkens and a cloud gathers over you, flashing from inside. Animation: you float slightly above the ground in a yellow-and-cyan aura, arcs of current jump between your hands, three glowing pips on the cloud count the strikes left, and each bolt leaves a scorched circle and sparks while the ball bounces away.
- **In each mode:** Team matches and Bomb: as written. Hot Potato: Lightning Slide is a long dash (escape or reach). Strikes target the potato holder and only stagger them for 0.3 s, without knocking the potato loose, and kicking it away during the 0.75 s ring dodges the strike. 1v1v1v1: targets the nearest rival ball carrier, never the AI keeper. Final: as written; dodging by dribbling matches the dribble-vs-slide rule. Basketball: a strike can't hit a player in the air (the ring waits until they land). Volleyball: strikes mark the rival about to touch the ball and knock it short. Hockey: the knocked ball travels 4 m on ice. Crates: the stagger follows the crowd-control rules.
- **Counterplay:** The 0.75 s ring is the tell: pass, shoot, dribble or step out. Stay more than 10 m away, or let a teammate hold the ball. The pips show the strikes left. A missed Lightning Slide leaves Tempest on the ground longer.
- **Balance:** Area pressure that rewards reaction, not an automatic steal: a skilled carrier dodges about 2 of 3 strikes. Value about 1.9 on 24 s. The 0.3 s stagger plus the 2 s immunity rule rules out chain stuns.
- **What changed:** Awakening: automatic bolts with no warning (12 m, 0.5 s stun × 3) → a dodgeable 0.75 s ring, 10 m range, a 0.3 s stagger, never keepers and no aura from strikes. Ability: 2× distance → +60% distance and +30% speed, with a miss penalty. Cooldown 29 → 24 s. Renamed Grom → Tempest, Awakening Thunderhead. Now electric yellow with cyan, so it no longer shares Snapshot's gold.

<details><summary>Awakening animation brief</summary>

Existing 'storm' fx, Legendary budget (sky and light change, letterbox, 60% flash, 0.12 s hit-stop, 10° punch). TELEGRAPH 0–1 s: letterbox bars slide in (two DOM bars in .cstage, 0 → 10% height). The hemisphere and sun lights drop to 30% while the tint sphere darkens toward #0A0C18. Five dark cloud puffs merge from four directions into the storm cloud 4.4 m above, and its inner point-light flashes speed up from every 0.3 s to every 0.1 s. 40 leaves and grass blades spiral up around the player, the hair spikes rise and a static crackle builds. BURST at 1.0 s: a giant bolt strikes the player, drawn in 3 layers with the jag() helper (white core, cyan glow, wide faint halo); 0.12 s hit-stop, a 60% white flash, an FOV punch of −10° with a +4° overshoot, and a 0.2 shake for 0.3 s. The player lifts 0.35 m into the twin aura cones, a scorch ring appears under the feet, and a 'THUNDERHEAD' banner appears. LOOP: three glowing pips sit on the cloud. At 1.5, 3.0 and 4.5 s a carrier dummy is marked: a yellow-cyan ring shrinks from 1.2 m to 0 over 0.75 s with a rising tone (the readable dodge window). The first strike hits: a bolt for 0.12 s, a point-light flash, 0.05 s hit-stop, a scorch decal and 20 sparks, and the ball bounces 3 m. The second is dodged: the dummy stepovers out and the bolt fizzles on an empty ring with a smaller spark. One pip goes dark per strike. Arcs jump between the hands (re-rolled every 0.07 s), 60 rain streaks fall within 10 m, and sheet lightning flashes the hemisphere light every 1.2–2 s. OUTRO (last 0.6 s): the cloud rains briefly, thins and shrinks, the light returns as if the sun breaks through, the letterbox slides out, and the player lands with a static ring. Palette #FFE14D, #7FE8FF, white core, cloud #3A3F55 on navy. Budget about 120 meshes.

</details>

<a id="mimic"></a>
#### Mimic  ·  Legendary  ·  copying

| Cooldown | Ability lasts | Awakening lasts | Chance per roll | Colours | 3D effect | Formerly |
|---|---|---|---|---|---|---|
| 26 s | 8 s | 4 s | 1.67% | `#E6E8F5` `#6A3DB8` | `cham` | Kameleon |

- **Ability (1):** Shadow Grab: a shadow tendril copies the ability of the nearest rival within 10 m. You hold it for up to 8 s and fire it with 1. It works at 80% of its numbers (durations, bonuses and radii), and firing it starts no extra cooldown. Copying another Mimic fails and refunds half the cooldown. Telegraph (0.5 s, rivals can see it): your body darkens and a pool of shadow spreads under your feet. Animation: a black tendril crawls from your shadow into the rival's shadow and pulls out a glowing orb of their ability. The orb floats to you wrapped in shadow and circles over your head until you use it, while dark smoke rises from the pool.
- **Awakening (G, 100 aura):** Stolen Shadow: copies the Awakening of the rival you're facing within 15 m (or of the nearest rival, if none is in front of you) and fires it at once at 75% of its duration, even if they haven't charged it yet. The rival keeps their own aura. The copy follows the copied style's rules for the current mode and all global caps. If the target is another Mimic, you get two full-strength Shadow Grab charges instead. Telegraph (1 s, rivals can see it): you melt into shadow, the target is outlined in violet and the icon of the Awakening you're about to copy appears over your head. Animation: the rival's shadow tears off their feet, glides over the ground and stands up behind you as a black double with a rim in the rival's colour. The copied Awakening plays in ink colours with violet edges so everyone can tell it's a copy, then the shadow slides back to its owner.
- **In each mode:** Every copy follows the copied style's rules for the current mode. Team matches: choose whom to stand near. Hot Potato: copies use their Hot Potato versions, and the last-10-s lock applies. 1v1v1v1: there are 3 possible targets; the AI keeper has no style and is never a target. Final: you always copy the other finalist, which makes it a mirror duel. Volleyball: copying works across the net. Basketball, Hockey, Bomb and Crates: no extra changes.
- **Counterplay:** Keep out of 10 m during the tendril telegraph to avoid the grab, and turn away or step out of 15 m when the violet outline appears; the icon gives 1 s of warning. Copies run at 75–80% strength, keep the original's telegraph and counterplay, and have violet edges.
- **Balance:** Its power is the lobby's average style at 75–80%, slightly below par, paid back by flexibility; it has the longest Legendary cooldown (26 s), value about 1.9. Every Awakening now shares one budget, so copying one before its owner has charged it is a sidegrade, not a theft of the best tool in the lobby.
- **What changed:** Ability: added a 10 m range, an 8 s hold, 80% strength and a Mimic-vs-Mimic rule. Awakening: a full-strength copy of anyone → 75% duration, aimed by facing, with the target warned by a violet outline and an icon; Mimic vs Mimic defined. Cooldown 30 → 26 s. Renamed Kameleon → Mimic, because a copy ability called Chameleon is tied to a famous soccer anime. New pale colour so it no longer clashes with Shade's purple.

<details><summary>Awakening animation brief</summary>

Existing 'cham' fx, Legendary budget. TELEGRAPH 0–1 s: the letterbox slides in and the scene desaturates 40%. The player's body desaturates and turns 50% transparent, and the shadow pool grows from 1 m to 2.5 m with an ink-spread edge. Five ink tendrils (tapered planes) race across the ground to the target dummy, which gets a violet outline (a BackSide scaled clone), and the icon of the Awakening about to be copied fades in over the head in the source style's colour. Ink droplets float upward like rain in reverse. Replace today's black spheres with purple-tinted, see-through smoke so it reads on the dark stage. BURST at 1.0 s: the rival's shadow tears off (a flat plane rotating from lying to upright) with 0.12 s hit-stop, a two-frame flash (dark violet, then 50% white), an FOV punch of −10° and a cloth-tear sound. It glides over in 0.25 s and stands up behind the player as the black double with a rim in the victim's colour, and the victim's Awakening banner appears with a violet 'STOLEN' stamp slammed across it. LOOP: the double mirrors the player 0.1 s late. The copied Awakening's own fx runs re-tinted: lerp its materials toward ink #2A0F3A, keep only the rims in the victim's colour, and add violet edges (in the viewer, copy Burst's Redline, which is cheap to re-run). Smoke wisps orbit and a violet countdown ring drains. OUTRO (last 0.6 s): the double collapses into a puddle, slides back across the ground and snaps onto the rival's feet like an elastic band with a small flash (the shadow is returned); colour returns to the body and the letterbox slides out. Palette #E6E8F5, #6A3DB8, ink #07030C, plus the victim's colour. Budget about 90 meshes on top of the copied effect.

</details>

<a id="analyst"></a>
#### Analyst  ·  Legendary  ·  tactics

| Cooldown | Ability lasts | Awakening lasts | Chance per roll | Colours | 3D effect | Formerly |
|---|---|---|---|---|---|---|
| 25 s | 5 s | 6 s | 1.67% | `#B6FF3A` `#E0E4F5` | `analyst` (to build) | Analityk |

- **Ability (1):** Readout: for 5 s you see, for every rival within 15 m, a small cooldown ring over their head showing when their ability is ready, and, if they are charging a shot or lob, their shot line (the dashed aiming line that normally only the shooter sees). Rivals you are reading see an eye icon over their own head. Telegraph (0.5 s, rivals can see it): you tap your temple and a thin lime scan line sweeps out over the ground. Animation: a holographic tablet appears in your hand, rivals in range get thin lime outlines with cooldown rings over their heads, and every shot line you read glows lime and ends in a target ring.
- **Awakening (G, 100 aura):** Game Plan, 6 s: you and teammates within 15 m see every rival's shot line, plus a short arrow showing which way each rival ball carrier is facing to pass. Rivals inside the 15 m area see the eye icon. In modes without teammates only you see it. Nothing is blocked or disabled: it is information only. Telegraph (1 s, rivals can see it): a holographic tactics board unfolds in front of you. Animation: a lime grid sweeps over the pitch from your feet, rivals get thin outlines, their shot lines and pass arrows glow, and small tactical markers float over your teammates.
- **In each mode:** Team matches: strongest with teammates who act on the information. Hot Potato: Readout shows the potato holder's kick line, so you can sidestep; Game Plan shows it to you only. 1v1v1v1: shows every rival's line (solo version); great for blocking. Final: solo version; the rival sees the eye and can fake or cancel with C. Basketball, Volleyball, Hockey, Bomb and Crates: as written.
- **Counterplay:** The eye icon tells you you're being read: cancel the shot with C and re-aim, fake a shot, or pass instead. The Analyst still has to get into the lane in time, and nothing is disabled.
- **Balance:** Pure information, and the rival always knows they're being read, so its value depends on skill: about 1.9 on 25 s for a player who reads well, much less for one who doesn't. It is the high-ceiling, low-floor Legendary. The draft's silence was removed because a 6 s ability lock is a hard counter that feels unwinnable.
- **What changed:** Hidden draft Analityk finished. Ability: 'see who has abilities ready' → rival shot lines plus cooldown rings within 15 m, with an eye-icon warning; seeing who has 100 aura is now a global ready glow for everyone. Awakening: the 6 s silence → a 6 s team read of shot lines and pass directions. Cooldown 30 → 25 s. New lime colour so it no longer looks like the grey Common frame. Needs a new fx.

<details><summary>Awakening animation brief</summary>

New fx 'analyst', Legendary budget. BUILD: a tactics board (three hinged 0.47 × 0.9 m planes with a grid CanvasTexture); X and O marks (thin box pairs and TorusGeometry); a grid-sweep ring (RingGeometry with a scrolling grid texture); rival dummies with lime wireframe rim boxes (BoxGeometry plus EdgesGeometry LineSegments); dashed shot lines (Line with LineDashedMaterial and computeLineDistances) ending in target rings; eye sprites from a CanvasTexture; chevron markers. TELEGRAPH 0–1 s: the letterbox slides in with a soft data hum. The board unfolds in front of the player in three hinged panels (0.15 s each, rotation.y ease-out), X and O marks draw themselves, a scan line sweeps out over the ground, and the camera lifts to a high angle. BURST at 1.0 s: the board flips down to the ground and becomes the grid sweep, expanding to 15 m in 0.4 s; 0.12 s hit-stop as it passes the dummies, which get lime rim outlines for 0.3 s and eye icons; a 60% lime-white flash, an FOV punch of −10° and a 'GAME PLAN' banner. LOOP: rival dummies charge shots one after another, and their dashed shot lines grow along the real curve (setDrawRange) to a target ring. A teammate dummy steps into the line and blocks, with a lime flash at the block point. Pass arrows hover at the carrier's feet, chevrons bob over teammates, eye icons blink, and a lime countdown ring drains. OUTRO (last 0.6 s): the grid reels back into the player like a tape measure, the board folds into a card that flips and vanishes, and the letterbox slides out. Palette #B6FF3A, #E0E4F5, accent #7CE3FF. Budget about 80 meshes.

</details>

### Mythic styles

<a id="hourglass"></a>
#### Hourglass  ·  Mythic  ·  time

| Cooldown | Ability lasts | Awakening lasts | Chance per roll | Colours | 3D effect | Formerly |
|---|---|---|---|---|---|---|
| 29 s | 5 s | 4 s | 1% | `#3D5BFF` `#A8D2FF` | `time` | Metawizja |

- **Ability (1):** Slow Drop: for 5 s, when the ball is in the air within 3 m of you, it moves at 70% speed. It's the real ball, so everyone sees and plays the same slowed ball. Your volleys off it are 15% stronger. Telegraph (0.5 s, rivals can see it): a small blue clock appears under your feet with its hand spinning wildly. Animation: your kicking leg glows blue, blue sparks circle your boot, a faint 3 m blue ring shows the slow area, and every volley leaves a blue streak behind the ball.
- **Awakening (G, 100 aura):** Still Hour, 4 s: a blue dome with a 7 m radius forms where you stand and stays there. Inside it, everyone except you moves at 70% speed, and so does the ball, except when you kick it (your kicks leave at full speed). Goalkeepers and anyone outside the dome aren't affected, and a player who leaves the dome is back to normal at once. Telegraph (1 s, rivals can see it): the clock hands under your feet spin faster and faster and loud ticking builds. Animation: a blue dome blooms from where you stand and a giant clock face appears on the grass inside it with crawling hands. Everything inside fades to blue-grey, dust and sparks hang in the air and slowed players drag afterimages, while the sky over the whole pitch dims for a moment (visual only).
- **In each mode:** Team matches and Bomb: as written; teammates inside are slowed too, so place it with care. Hot Potato: the dome is 4 m and the round clock never slows; Slow Drop slows a potato in flight near you, giving you time to dodge or set up a kick. 1v1v1v1: the dome can't form within 8 m of the goal (it forms on the 8 m line instead), and the AI keeper is never slowed. Final: as written; the rival can walk out of a static dome. Basketball and volleyball: the ball slows inside the dome for everyone, and the volleyball landing ring stays accurate. Hockey: slowed players keep sliding. Crates: no interaction.
- **Counterplay:** The dome doesn't move: leave it during the 1 s telegraph or walk out of it, and it lasts only 4 s. Pass the ball out of the dome. Keepers are immune, and Hourglass itself is no faster than normal. Rivals can volley the slowed Slow Drop ball too.
- **Balance:** Mythic buys spectacle, not power: its numbers sit on the Legendary budget (value about 2.2 per use on 29 s, the same per minute as every tier). A local, static zone at 70% replaces a pitch-wide time stop at 40%, so rivals can always act and escape.
- **What changed:** Legendary → Mythic: the single chase style. Awakening: everyone on the pitch slowed to 40% for 5 s → a static 7 m dome at 70% for 4 s, with keepers immune and an 8 m goal rule in Match 5. Ability: 1.5× volley power (which overlapped with Acrobat) → a local ball slow plus +15% volleys; 4 s → 5 s. Cooldown 28 → 29 s. Renamed Metawizja → Hourglass, because 'Metavision' is a signature term of a famous soccer anime; the Awakening name Still Hour avoids 'Time Stop', a famous anime power.

<details><summary>Awakening animation brief</summary>

Existing 'time' fx, Mythic budget (the whole scene transforms: 0.2 s hit-stop plus a slow-motion ramp, a 12° punch, letterbox, replay camera). TELEGRAPH 0–1 s: the ticking accelerates (interval 0.25 s → 0.05 s) and the mini clock spins up (hand speed 1 → 12 turns per second). 12 hour-mark bars rise from the turf in a 2 m ring and start orbiting, three brass gears (TorusGeometry plus tooth boxes) fade in behind the player, counter-rotating, and the letterbox slides in. The camera orbits 30° and rises. At 0.8 s all sound ducks (low-pass), and at 0.9 s the hands stop at 12 and everything freezes for 0.1 s. BURST at 1.0 s: a deep bell strike. A full 0.2 s hit-stop, then 0.4 s at 30% speed (scale the viewer's dt) while the dome (wireframe icosahedron plus a soft fill, with a brighter leading rim) expands 0 → 7 m and the giant clock face, scaled to the 7 m radius, draws its 12 ticks around. A 60% white-blue flash, and an FOV punch of −12° that slowly widens to +6° as the dome grows. The inside colour-grades to blue-grey (tint plus the existing dummy colour lerp) while the sky tint dims 20% everywhere, motes freeze in place, and a 'STILL HOUR' banner with a clock icon appears. LOOP: dust, sparks and 40 water droplets hang in the air; slowed dummies drag 3 afterimages; the ball inside shows a visibly slowed spin and a blue trail. The second hand ticks every 0.5 s with a heavy 'tock', and a ripple ring slides over the dome each time. The player alone stays full colour and leaves a blue time-trail, and one big second hand sweeps the face once over 4 s as the countdown. OUTRO (last 0.5 s): the hands spin backwards fast, the dome cracks like glass along the wireframe and shatters into blue shards, the frozen particles snap back into motion, colour floods back, the letterbox slides out, and one last soft tick. Palette #3D5BFF, #A8D2FF, blue-grey #8C9BB0, brass #C9A24A. Budget about 130 meshes.

</details>

## 8. Rolling

**What a roll does**

1. In the lobby, pick one of your style slots and pay 100 coins (or use a free roll).
2. The game first draws the rarity: Common 52%, Rare 28%, Epic 14%, Legendary 5%, Mythic 1%, changed only by the guarantees (section 9).
3. Then it picks one style inside that rarity with equal chance: each Common 7.43%, each Rare 5.6%, each Epic 3.5%, each Legendary 1.67%, the Mythic 1%.
4. A reel spins for about 3 s (skippable after 1 s), flashes the rarity colour and flips the style card, which plays a short clip of its Awakening. The reveal grows with the rarity: a quiet flip for Common, a flash and sparks for Rare and Epic, light rays and a server announcement for Legendary, and letterbox bars, a pulse and the biggest burst for Mythic.
5. You choose **Keep new** (the new style replaces that slot's style) or **Keep current** (the slot stays as it was). The cost is spent either way, so a roll can never take away a style you like by accident. An empty slot keeps the new style automatically.

The roll screen always shows the full odds table (per rarity and per style), the three guarantee counters (for example "Epic+ guaranteed in 6") and any odds the guarantees have changed. Legendary and Mythic rolls are announced to the whole server.

**Slots:** slot 1 from the start; slot 2 at account level 5 or for 1,500 coins; slot 3 at level 15, for 4,000 coins or for 149 Robux. Each slot has a free lock toggle, and a locked slot can't be picked for a roll. Slots are storage: more slots let you keep more styles to choose from, but never add power in a match.

**One style per match:** before a tournament starts, pick your active slot in the lobby (keys 1/2/3 select it). That style is locked for all 7 games and shown on your intro card, so rivals can learn it. Every style has a written version for every mode, so none goes dead. Rolling only happens in the lobby, never during a tournament.

**Keys in a match**

| Action | PC | Phone | Gamepad |
|---|---|---|---|
| Your style's ability (cooldown) | 1 | small button 1 | D-pad up |
| Awakening (100 aura) | G | glowing Awakening button | LB + RB |
| Use the power-up crate you carry (Match 3 crate mode; empty otherwise) | 2 | small button 2 | D-pad right |
| Comms wheel: a tap sends a "Pass!" or "Shoot!" ping, holding opens pings and emotes | 3 | small button 3 | D-pad left |

So every style is exactly one ability plus one Awakening, and keys 2 and 3 never hold another ability.

**Bots** fill empty places with random styles from the same table, except the Mythic (for bots its 1% goes to Common), so an Hourglass on the pitch is always a real player.

**Plan-page demo:** the 🎲 Roll button opens the roll panel with 3 open slots, 300 coins and 1 free first roll (guaranteed Rare or better). Each roll costs 100 coins (or uses the free roll) and shows the reel, the result card, Keep new / Keep current, the three guarantee counters and the odds table. "Play a tournament" adds a random payout (20–150 coins by finish, plus a small performance bonus), "+ 500 coins (demo)" tops up, and "Simulate 1,000 rolls" runs the real roll code to compare observed and expected odds. Randomness comes from `crypto.getRandomValues`, and the demo state is saved only in that browser.

## 9. Guarantees (pity)

Three counters, always visible on the roll screen. They are shared by coin rolls, free rolls and any Robux spins, and never reset over time.

| Guarantee | When | Result on that roll |
|---|---|---|
| Epic or better within 10 rolls | after 9 rolls in a row of Common or Rare | Epic 70%, Legendary 25%, Mythic 5% (the same ratios as the normal table) |
| Legendary or better within 50 rolls | roll 50 without one | Legendary 83.3%, Mythic 16.7% |
| Mythic within 200 rolls | roll 200 without one | the Mythic |
| Free tutorial roll | the very first roll | Rare 58.3%, Epic 29.2%, Legendary 10.4%, Mythic 2.1% |

- Hitting a tier resets its counter and every counter below it: a Legendary resets the Epic and Legendary counters, and the Mythic resets all three.
- A guaranteed result never gives a style you currently hold in a slot.
- Whenever a guarantee changes the odds, the roll screen shows the updated odds before you confirm.
- Effective odds including the guarantees (2-million-roll simulation): Common 50.3%, Rare 27.1%, Epic 15.6%, Legendary 5.7%, Mythic 1.3%.
- On average it takes about 4.4 rolls to get an Epic or better, about 14 to get a Legendary or better and about 79 to get the Mythic; 13.5% of players need the full 200 for the Mythic.

## 10. Duplicates and the Style Book

- Every style you roll is recorded in your **Style Book**.
- A result that matches a style already in one of your slots is a **duplicate**: the slot stays as it is, you get coins back and the style gains 1 **Mastery Star**.

| Duplicate of | Coins back |
|---|---|
| Common | 25 |
| Rare | 40 |
| Epic | 60 |
| Legendary | 100 |
| Mythic | 200 |

- Rolling any style you've rolled before also gives it a Star, even if it isn't in a slot now.
- Guaranteed (pity) results never give a duplicate, and duplicates count toward the guarantee counters.
- Stars (max 5) are cosmetic only and stay in the Style Book even after you roll the style away: 1 = alternate colour palette, 2 = Awakening banner on the scoreboard, 3 = alternate Awakening burst colourway, 4 = animated nameplate, 5 = gold version of the Awakening animation.
- No cosmetic ever changes a telegraph's shape, a range ring or any other gameplay tell, so rivals always read a style the same way.
- Completing a rarity row in the Style Book pays Common 200, Rare 300, Epic 500, Legendary 1,000 coins.

## 11. Coins: how players earn rolls

Coins are earned only by playing and are never sold for Robux (not even through bundles or multipliers).

**Per tournament (about 12 minutes), by how far you get:**

| Finish | Coins |
|---|---|
| Out in Match 1 or the Wild Card | 20 |
| Out in Match 2 | 30 |
| Out in Match 3 | 40 |
| Out in Hot Potato | 55 |
| Out in Match 5 | 75 |
| Runner-up | 100 |
| Champion | 150 |

- The Wild Card winner gets +10.
- Performance bonus: goal +2; assist, save or tackle +1; at most +20 per tournament.
- **Anti-farm:** you only earn coins if you finish Match 1 without going AFK (at least 3 ball touches). Goals against bot-only teams pay half, and private servers pay half.
- Smaller tournaments (8/16/24 players with bots) pay the same coins per minute, because the payout scales with the stages actually played.
- An average player earns about 40 coins per tournament (about 200 per hour).

**Daily and weekly:**

- Daily Kickoff: +100 for your first finished tournament.
- 3 daily quests at 40 coins each (for example score 2 goals, win 3 tackles, reach Match 3).
- From day 2: 1 free roll per day after you finish a tournament.
- Weekly: finish 15 tournaments for +300.

**New players:** 300 starting coins, a free tutorial roll (guaranteed Rare or better), and welcome quests worth +100 each (finish the tutorial, finish your first tournament, reach Match 3 for the first time). A typical first session (3 tournaments, about 40 min) gives about 700–850 coins plus the free roll, i.e. 8–9 rolls, with an 88–90% chance of at least one Epic or better and a 43–47% chance of a Legendary or better. A daily player gets about 5 rolls a day.

**Other coin sinks:** slot unlocks, the Style Shop and coin cosmetics.

## 12. Robux and the rules for paid random items

**At launch: no paid random items.** Coins are never sold for Robux, so coin rolls stay outside Roblox's paid-random-item rules (the odds are shown anyway). Robux buys only fixed, non-random things:

1. **Style Shop:** 4 specific styles that rotate every 24 h, the same for everyone and shown a day ahead: one Common, one Rare, one Epic and one Legendary; the Mythic appears every 14 days.

   | Rarity | Robux | Coins |
   |---|---|---|
   | Common | 75 R$ | 600 |
   | Rare | 150 R$ | 1,200 |
   | Epic | 300 R$ | 2,500 |
   | Legendary | 600 R$ | 5,000 |
   | Mythic | never for Robux | 10,000 |

2. **Slot 3** for 149 R$: storage only, and also earnable.
3. **Cosmetics**, the main earner: Awakening colourways, trails, banners, goal celebrations and emotes, which never change gameplay tells.

Every tier has the same power budget and only one style is active per match, so money buys choice and spectacle, not strength.

**Optional later, only if you decide to add paid rolls:** Style Spins at 1 for 40 R$, 5 for 180 R$ and 12 for 400 R$. A spin works exactly like a coin roll: the same odds, the same shared guarantee counters and the same Keep new / Keep current choice. Compliance:

- The full odds table (per rarity and per style, plus the current guarantee-adjusted odds) appears on the roll screen and on the purchase prompt before any Robux is spent.
- Spins are offered only when `PolicyService:GetPolicyInfoForPlayerAsync(player).ArePaidRandomItemsRestricted` is false; restricted players see only coin rolls and the shop.
- Styles can never be traded.
- A spending reminder appears after 60 spins bought in 30 days.

## 13. Awakening animation upgrade

### What's wrong today

- Telegraphs look alike: the generic cubes plus a spotlight cone make Burst, Groove, Acrobat, Shade and Ember (formerly Zryw, Taniec, Akrobata, Cień and Feniks) nearly identical.
- Some Awakenings don't read at all:
  - Mimic and Shade are dark spheres on a dark stage.
  - Acrobat is only a spotlight until a volley happens.
  - Ember's wings are too thin to see.
- No Awakening has a burst moment (flash, shockwave, camera punch, hit-stop) or a clear ending, and rivals get no "time left" signal.

### A shared kit (the "Awakening Director"), built once in the viewer

- **Timeline:** the telegraph runs from 0 to 1 s, the burst lands at 1 s, then the loop, and the outro takes the last 0.5–0.6 s. Each phase exposes a 0–1 progress value.
- **`hitStop(s)` and `slowMo(scale, s)`:** feed every effect update a scaled `dt`.
- **`punch(deg, s)`:** offsets the camera FOV and eases back.
- **`shake(amp, s)`:** a single decaying shake that respects reduced motion.
- **`flash(color, alpha, s)`:** an additive plane parented to the camera.
- **`banner(name)` and `letterbox(on)`:** DOM elements in `.cstage`.
- **`countdownRing(color)`:** a ground ring that drains with the remaining time.
- **Pools:** rings, sparks, puffs and debris, through the existing emit/tick helpers.
- **Per style:** hide the generic shell cones, and keep the converging cubes only as a tinted base layer under the style's own telegraph.

### Presentation budget by tier

| Tier | Effect space | Flash | Hit-stop | FOV punch | Extras |
|---|---|---|---|---|---|
| Common | about 3 m plus a trail | local sprite | 0.05 s | 4° | none |
| Rare | up to 7 m | 25% screen | 0.06 s | 6° | name banner |
| Epic | a summoned set piece | 40% | 0.08 s | 8° | banner, music sting |
| Legendary | sky and light change | 60% | 0.12 s | 10° | letterbox |
| Mythic | the whole scene | 60% | 0.2 s plus a slow-motion ramp | 12° | letterbox, replay camera, server-wide chime |

**New effects to build:** swerve, cannon, tower, playmaker, hawk, analyst. Until they exist, those six styles show the generic aura in the viewer (the `FXK` list in the viewer script says which effects are built). The other 14 styles reuse and upgrade their existing effect (speed, dance, time, king, cham, acro, wall, ninja, impact, wolf, ice, magic, phoenix, storm). The animation brief for each style is in section 7, under "Awakening animation brief".

## 14. What changed on the plan page

- `CLS` now has five rarities (Common, Rare, Epic, Legendary, Mythic) with drop rate and cooldown band; `CH` holds the 20 styles in English with `da` and `dw` set from the durations above, plus short summaries, mode notes, counterplay, balance notes and what changed.
- The Polish status badges (settled / new idea / draft / skipped) are gone: every style is shown, and the badge now shows its exact chance per roll.
- The counter comes from `CH.length` (1 / 20). The legend lists the rarities with drop rate and cooldown, and tapping a rarity jumps to it.
- Styles whose effect isn't built yet fall back to the generic aura instead of a blank stage.
- Rules: new sections 11 (styles, rarities and rolling) and 12 (fair-play rules), the lobby style pick, the crate key, and the open questions below. Controls: keys 1/2/3 and G, the dribble guard, the aura caps and carry-over.
- New 🎲 Roll button and roll dialog (see section 8).

## 15. Open questions

1. **Keys 1/2/3:** the proposal is one active style per player (1 = ability, G = Awakening, 2 = Match 3 crate item, 3 = comms wheel). Do you want this, or three styles in a match with one shared cooldown?
2. **Style lock:** the active style is locked for the whole tournament (proposed, for readability and no counter-picking). Or may players switch in the Hall of Winners between matches, for example after the Match 3 mode is revealed?
3. **Roll model:** rolling into a slot with Keep new / Keep current (proposed), or a permanent Collection where every rolled style is unlocked forever and can be equipped freely?
4. **Goalkeepers** in team matches and in the Final: AI keeper, player keeper or none? This changes Bulwark's 8 m rule, Shade's goal-area rule and the Cannon and Snapshot numbers.
5. **Base numbers in studs:** walk, sprint and with-ball speed, slide length (about 4 m assumed), tackle and interception reach (1.5 m), full shot charge (1.0 s) and jump height. All percentages and caps are relative to these.
6. **Standing tackle:** is there a standing tackle or poke, or can the ball only be won by a slide tackle or an interception? The counterplay for Groove and Monarch depends on it. Do slide tackles exist in the Hot Potato chamber?
7. **Aura:** agree that up to 50 aura carries into the next match and that both finalists start the Final with at least 50?
8. **Hot Potato:** Awakenings are allowed except in the last 10 s of a round. Aura there: +10 for kicking the potato into a rival, +5 for a dodge, +10 for surviving a round, max 30 per round. Does a header count as a kick?
9. **Mythic tier:** keep Hourglass alone at 1% (proposed), or fold it into Legendary (Legendary 6% with 4 styles)?
10. **Robux:** launch without paid random spins (proposed), with Robux buying only specific styles from the daily rotating shop (75/150/300/600 R$; Mythic coins only), slot 3 and cosmetics? Should Legendaries be buyable with Robux at all?
11. **Coin pace:** 100 coins per roll, about 40 coins per tournament plus daily bonuses, about 5 rolls a day for a daily player and 8–9 in the first session. Is this the pace you want? Should there be a 10-roll bundle?
12. **Bots:** they use the same odds but never get the Mythic. Should bots in small lobbies lean toward Common and Rare so new players don't feel outclassed?
13. **Trading:** none (proposed), to avoid a gambling-like secondary market. Confirm.
14. **Seasons:** add new styles from the bottom of the pyramid up, while each tier's rate stays fixed and only the per-style share drops. Is Ricochet (bank shots off walls, a Rare from the spectacle proposal) a good first candidate?
