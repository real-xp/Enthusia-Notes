# Enthusia: Professional Racing Speedrunning Guide
A (somewhat) comprehensive guide on how to speedrun this quirky 20-year old simcade by mrxbas and Azullia

Original document is at https://docs.google.com/document/d/1jX8PJZ2lZpuxxPWasJbqe8-HDl9FXBfmXOCUV5JHRO0 (maybe more updated)

# Table of contents
- [Enthusia: Professional Racing Speedrunning Guide](#enthusia-professional-racing-speedrunning-guide)
- [Table of contents](#table-of-contents)
- [Why should you speedrun Enthusia?](#why-should-you-speedrun-enthusia)
- [Setting everything up](#setting-everything-up)
- [The speedruns](#the-speedruns)
  - [Rank1%](#rank1)
  - [Any% {#any%}](#any-any)
  - [Driving Revolution All C%/S%](#driving-revolution-all-cs)
- [Additional information](#additional-information)
  - [Ranking guide](#ranking-guide)
    - [League Qualification Thresholds](#league-qualification-thresholds)
    - [Base scores per league](#base-scores-per-league)
    - [Finishing position multipliers](#finishing-position-multipliers)
    - [Routes \& RNG Manipulation](#routes--rng-manipulation)
  - [Other Resources](#other-resources)

# Why should you speedrun Enthusia?

Enthusia: Professional Racing is a racing game released by Konami in 2005, a few months after Gran Turismo 41. It features simcade handling (though it’s closer to a sim-like, especially for the time), generally giving great but difficult to master driving physics, as well as a mostly non-linear campaign in its “Enthusia Life” mode. 

1 Probably the reason why people often call it a “Gran Turismo clone” as well as the fact it didn’t sell very well.

Generally, outside of [single events and/or tracks](https://youtu.be/Y99Wj-NStok), racing games have not been very popular for speedrunning, either because they are too linear, consistent or (like Gran Turismo 4), [too long](https://www.speedrun.com/gt4). However, Enthusia somewhat stands out in that regard, because:

* Speedrunning Enthusia is relatively **straightforward and approachable**, as there are only a few key things you have to remember, and there aren’t any major bugs or glitches to be abused. (Yet.)  
* The game has multiple things that are RNG-dependent in various levels, while most often allowing driver skill and track knowledge to defy the odds (literally\!)  
* There is a sufficient amount of **variety** in speedrunning this game, coming from both the RNG factor and the runner’s choice of car and event, often requiring you to make **on-the-fly risk vs. reward decisions**.  
* Despite our attempts to optimize most of the run, there may very well be certain conditions that could **allow for faster routes**.  
* All routes of the game do not take too long to complete (usually **between 1 and 3 hours**), and (with an exception for one part of Any%), **free of tedium**.  
  * Maybe do not do a Driving Revolution category as your first ever speedrun of this game, though.

If you’ve read this far, I think you are probably interested enough in looking to speedrun this game, so it’s time to read the next part already.

# Setting everything up

Enthusia can be speedrun through both [Speedrun.com](https://www.speedrun.com/epr) and [RetroAchievements](https://retroachievements.org/game/21944?view=leaderboards)2,3. For both, all you really need is an account for their respective platform and a way to time your runs. In the case of Speedrun.com, this can be done through a timing app like [LiveSplit](https://livesplit.org/) (set to millisecond precision) while recording your run (e.g. through OBS), while for RetroAchievements, you only have to log in on the emulator (in PCSX2: settings → Achievements). It is also recommended to turn off **Impact Flash**, as that slows down the game with not much gain.  
2 It is recommended to enable RetroAchievements' **Hardcore Mode**, as that is what enables leaderboard eligibility for RetroAchievements, and also disables emulator slowdown features, save state usage and external/unapproved patch loading.  
3 Some timings for some categories are different between Speedrun.com and RetroAchievements. You will need a separate timer for RTA times when submitting to Speedrun.com.

Enthusia has three main speedrunning categories:

* [Any%](#any%): beat the final race of the year and reach the end credits.  
* [Rank 1%](#rank1%): reach first place in the in-game ranking leaderboard.  
* [Driving Revolution, all C%/S%](#driving-revolution-all-c%/s%): complete all Driving Revolution missions with at least a passing grade (C%) or S-rank (S%).

All of the above categories have leaderboards on Speedrun.com and RetroAchievements and can therefore be run simultaneously, though do note that the timer provided in an emulator for RetroAchievements compensates for emulation speed, so will usually show lower times than a real-time speedrun due to possible lag.

Before starting any runs, ensure all the controls are bound to what you'd like in-game, and ensure you **disable ESC and TCS in the Driver settings**, as having them on reduces the car and driver EXP you gain from races.

# The speedruns

## Rank1%

A Rank1% speedrun's goal is relatively straightforward. Reach Rank 1 in the in-game leaderboard as fast as you can. The one thing to watch out for is that **the timer starts on week 2, upon pressing continue from the Enthusia Life submenu**.

1. Start new games with your chosen starter car until you find a race within RN that has the **highest odds multiplier you can find**. Tune your car and save the game after you have found your desired race.  
2. Win that race. Critically, win that race **flawlessly** without losing any Enthu Points, in order to maximize the amount of car and driver EXP you will carry forward to the rest of the run. **Do not save the game yet at this point**.  
3. Check the next available races for Week 2\. If none of them have good odds (or if you think there may be other races available with higher odds for your car), load your save from the main menu and **redo step 24 and this step** until you find a race with the odds you want.  
4. **Save** the game here, and back out into the New Game / Continue menu of Enthusia Life.  
5. Press Continue. **Start your timer at the same time as you press Continue.**  
6. Progress through the career. Aim for races that **maximize the odds for the amount of time spent (while also not overshooting the Rank 1 requirement too much in points)**. Some tracks will be better than others, and you may have to **rest** to skip weeks to find better races.  
   * Should you drop into 5th or 6th place in a 6-car race, or 2nd place in a 2-car race, and you see you cannot get back your positions, it is faster to retire from the race and advance to the next one.  
7. **Stop your timer once you reach Rank 1 on the Ranking evaluation screen.** Wait until the animations finish and the text that shows how many positions you have gained has appeared before you stop the timer.

4 You can set up a **savestate** right before the end of the first race to reroll for the best possible odds for your second race. If you're aiming for an entry into the RetroAchievements leaderboards, remember to re-enable Hardcore Mode before starting your run.

You will not be switching cars in this run, as the starter cars will (usually) have better odds than any other car you come across, and **the upgrades obtained on the car from levelling up do not reduce odds**, so you'll be winning higher-level races with an upgraded starter car a lot easier than if you were switching cars. Switching cars also advances a week, which could cause you to lose Ranking Points.

You will need approximately **10800 to 11000** Ranking Points to reach Rank 1, so use this to optimize the last one or two races you will need to do.

Possible Starter/Main Cars:

* **Honda Beat**  
  * The current WR holder for this category. Gets very strong odds (in between the other two options) while being a blisteringly quick car around the corners, easily outmaneuvering most other starter car choices. It likely has the best potential of possible Rank1% times due to it being able to complete races much faster than the eK wagon, but its very momentum-dependent nature will demand the most of the driver, especially in the higher leagues. Its lack of power (it’s a kei car, duh) also makes it more vulnerable to bad RNG if the game generates a lot of races with powerful cars on non-ideal tracks.  
* **Toyota Estima Hybrid**  
  * The single fastest starter car in a straight line, and is also the only 4WD starter car, while also still maintaining relatively good odds values compared to the other starter cars due to its high mass. Generally has the most longevity and race options as it can compete and win in RI races more consistently than the others, while also being the most beginner-friendly option.  
* **Mitsubishi eK Wagon**  
  * Easily gets the highest odds multipliers of the starter cars, especially getting absurdly high odds when reaching higher leagues. This is, however, offset by its outright miserable specs: not only does it have just 50 BHP (among the lowest of any starter car\!). It also has a very awkward 3-speed automatic gearbox (configured like a 2+1 gearbox as the 3rd gear is set up similarly to an overdrive gear that is very far away from 2nd gear). Generally the hardest starter car choice, as various RII/RI races are outright impossible to score well on with this car, and the least recommended option from our testing.  
* **Nissan March / Micra**  
  * This car gets odds comparable to the Beat while being nearly as fast as the Estima in a straight line, making it a great choice for this run. You will need to get used to a little FWD understeer, but it is otherwise a very competitive car.  
* **Mini Cooper 1275S Mk. I**  
  * This car gets odds better than the Beat and Micra, but is faster than the Beat in a straight line (though slower than the Micra due to worse tires). It has the second highest odds potential right behind the eK, while being much nicer to drive overall.  
  * The higher odds on this car can also lend to a strategy of doing 3x RN races to get to RIII instead of having to do 2x RN and 1x RIV, which saves a minute.  
* **Polo**  
  * This car gets odds better than the Mini, but is much slower than it (only marginally faster than the eK).

You can find setups for these cars (as well as some general tuning tips for anything else you may find) in [this quick tuning guide](https://docs.google.com/document/d/1uSfw1M4AwCnkd-VSn6UspavUmq7nnC9nlKiVj8PUKOA/view).

We have tested the other 9 starter cars, but they were all either too slow to consistently win, or received low odds, which will slow down your climb to Rank 1\.

Preferred Tracks per League:

* **RN**: The 2 lap races on Autumn Hill or Marco Strada (\~2 minute races). However, Rev City is preferred for the Week 1 race you are forced to do (if you can find a race on it with high odds), as it will give you a boost of extra car EXP for faster possible upgrade levels.  
* **RIV**:  The 2 lap races on Rev City, or 3 laps of Autumn Hill or Marco Strada (\~3 minute races).  
* **RIII**: Any race that takes around 4 minutes or less (e.g. 4 laps of Victoria Garden, 2 laps of Victoria Road)  
  * 3 laps of Autumn Mountain and 5 laps of Autumn Hill are slightly worse at \~4.5-5 minutes each.  
* **RII**: The 2 lap races on Pacific Gateway, or Dragon Range (\~4 minute races).  
* **RI**: The 3 lap races on Pacific Gateway, or the 2 lap races on Löwenseering (if using the Estima; \~6 minute races), or Dragon Range (\~4 minute races, if the odds favour you).  
* **RS**: There are only really two RS races worth gunning for in a Rank1% speedrun:  
  * RWD Championship \#2 on April Week 2, though it is only worth attempting if it is **11 laps of Victoria Garden**6 **(even more so if it is the reverse version)**. It is however an 11-12 minute commitment, so be very careful.  
  * Dragon Festival on May Week 1, though you will likely need to hope for a downhill race (as it is much easier to catch up to and keep behind harder AI). If this race generates as an uphill race, it is a lost cause.

However, feel free to go onto a different track if you find that the odds value of that race far outweigh the extra time spent on the track, or to go for a safer option if you think you will not be able to win a race, since even getting P2 will lose you 40% of the available Ranking Points able to be gained from that race.

Ideally, you want to spend **exactly one, and no more than two races** in RN5 through RIII (so optimally, you want to reach RII in three races). This is relatively consistent for the Beat and eK, but the Estima might need an additional race as its odds are generally lower. From that point on, focus on **exclusively** racing in RI and RII, with the latter being a fallback option when there’s nothing in RI but a favorable Pacific Gateway race in RII.

You may need to Rest to skip races if no ideal RII and RI races are available, and you can skip about 3-7 weeks in a full Rank1% run before you start losing significant points from your races becoming older than 12 in-game weeks; 3-5 weeks if you'd like to min/max your run.

5 You leave RN and qualify for RIV by either completing five RN races, or winning 2 of them. The latter is preferred.  
6 Strategy first discovered by ImaSiphy ([SRDC](https://www.speedrun.com/users/imasiphy) / [RA](https://retroachievements.org/user/imasiphy)).

A premade save for Rank1% with a setup Honda Beat with a 13.3 01/01 Week 1 RN race finished can be found [here](https://www.speedrun.com/epr/resources/mv3jw).

## Any% {#any%}

An Any% speedrun's goal is to qualify for and win the King of the Year (KotY) Grand Prix race that occurs every 4th Week of December in each in-game year (and of course, winning the first year's KotY is the fastest available route). Unlike the other RS races in the game which only require you to be in the top 50 to qualify, the KotY race only allows the **top 6 (Rank \#1-6)** of a year to compete. **The timer starts upon pressing New Game, and ends when you win the KotY race** (when **CONGRATULATIONS** appears on your screen upon crossing the finish line on lap 2 in P1).

1. Start a new game and pick your chosen starter car. You will be sticking with this car for most of the run, so tune it with your desired setup before racing.  
2. Enter any race in 01/01 Week 1, and immediately press **RETIRE** on the pre-race menu (where START, REPLAY (grayed-out) and RETIRE are visible)7.  
3. Skip to either late August (Week 2-4) or somewhere in September (Weeks 1-4) in the in-game calendar by selecting the rest option repeatedly.  
   * If you are using a starter car with a selection for AT or MT, **select AT (the left option)**. There is a weird quirk where selecting Rest in the menu **opens the confirmation on OK instead of BACK when using AT**, which saves about 1-2 seconds per rest performed. You can change back to MT after skipping by selecting **Garage \> Other \> \[Your Car Color\] \> MT (the right option)**.  
   * You can pre-move the menu option the moment you see a line coming in from the right across the middle of the screen for faster skipping.  
4. Similar to a Rank1% run, progress through the career. Aim for races that **maximize the odds for the amount of time spent**. Some tracks will be better than others, and you may have to **rest** to skip weeks to find better races. **Aim for your rank to be between 1 and 5 before December Week 3\.** Try to also aim for races with a good car (used for KotY; most A- or B-class cars work, along with some of the faster C-class cars), as if you finish in at least 4th place, you'll have a chance to win it.  
   * Although KotY allows Rank 6 to enter, it is safer to get to Rank 5 or higher (\~9000 points) in order to ensure you do not fall out of the top 6 when skipping weeks towards December Week 4\. If you would like to optimize your usage of skips (resting), start going for the high-scoring races on and after **September Week 4**.  
   * Similarly to a Rank1% run, should you drop into 5th or 6th place in a 6-car race, or 2nd place in a 2-car race, and you see you cannot get back your positions, it is faster to retire from the race and advance to the next one.  
5. Change into your obtained KotY-contender car no later than December Week 3, then if need be, skip to December Week 4\. Make sure you apply any tuning settings it needs before entering the KotY race.  
6. Try your absolute best to win the KotY race. Use every dirty tactic and line (including any cutting or walltaps) to gain an edge, as your Enthu Points running out does not disqualify your win should you get it.  
   * Sadly, unless you're also coming in with your own supercar, some KotY lineups are almost impossible to beat (e.g. if a Ford GT, SLR or any RUF spawns as the fastest opponent).  
     * The Aston Martin V12 Vanquish, however, is very possible to beat.

7 Strategy first discovered by ImaSiphy ([SRDC](https://www.speedrun.com/users/imasiphy) / [RA](https://retroachievements.org/user/imasiphy)).

You will be using your starter car until it is time to switch cars for KotY (for similar reasons to Rank1%, where the upgrades on your car let it win more races while keeping their odds values). Race cars, should you obtain them in some way during this run, are useless. They can only race against other race cars, and the good race cars do not get good enough odds to climb ranks fast enough, and the bad race cars can barely win any races. The King of the Year Grand Prix also disallows race cars completely.

Possible Starter/Main Cars:

* **Honda Beat**  
  * This car gets very strong odds while being a blisteringly quick car around the corners, easily outmaneuvering most other starter car choices. It likely has the best potential of possible Any% times due to it being able to get much higher odds than the Estima, but its very momentum-dependent nature will demand the most of the driver, especially in the higher leagues. Its lack of power (it’s a kei car, duh) also makes it more vulnerable to bad RNG if the game generates a lot of races with powerful cars on non-ideal tracks.  
* **Toyota Estima Hybrid**  
  * The single fastest starter car in a straight line, and is also the only 4WD starter car, while also still maintaining relatively good odds values compared to the other starter cars due to its high mass. Generally has the most longevity and race options as it can compete and win in RI races more consistently than the others, while also being the most beginner-friendly option. It is also good for performing wallbounces or blocking.  
* **Nissan March / Micra**  
  * This car gets odds comparable to the Beat while being nearly as fast as the Estima in a straight line, making it a great choice for this run. You will need to get used to a little FWD understeer, but it is otherwise a very competitive car.  
* **Mini Cooper 1275S Mk. I**  
  * This car gets odds better than the Beat and Micra, but is faster than the Beat in a straight line (though slower than the Micra due to worse tires). It has the second highest odds potential right behind the eK, while being much nicer to drive overall.  
  * The higher odds on this car can also lend to a strategy of doing 3x RN races to get to RIII instead of having to do 2x RN and 1x RIV, which saves a minute.  
* **Polo**  
  * This car gets odds better than the Mini, but is much slower than it (only marginally faster than the eK).

You can find setups for these cars (as well as some general tuning tips for anything else you may find) in [this quick tuning guide](https://docs.google.com/document/d/1uSfw1M4AwCnkd-VSn6UspavUmq7nnC9nlKiVj8PUKOA/view).

Unlike Rank1%, the eK Wagon is not really an option, as you will need to prioritize staying in at least 4th or higher in some (six-car) races in order to get access to the Prize Car Raffle, to try and score a car that you can use to win KotY. Its lack of longevity also completely kills its potential for the late-game, as stronger opponents (which you could then get as potential prize cars) become practically inaccessible.

The track preference and rank optimization during your route is almost exactly the same as Rank1%, though you'll only need \~8850 points to secure Rank \#5-6. Though, Rev City and Mystic Caveway are less preferred for RN due to the fact that it loses a whole minute over Marco Strada and Autumn Hill. If you'd like to hyper-optimize your points gain, consider only the period between 01/09/W4 and 01/12/W3 as significant for your points total towards qualifying for the KotY race.

Unlike Rank1% however, there are two RS races that you can encounter within November that are viable for the speedrun, as they are (at most) not that much longer than the races you are normally competing in, and have the chance to award some powerful cars that can nearly guarantee a win in KotY:

1. **The Night Winding Championship** (November Week 1; Dragon Range; \~4 minute race): If you start early in August or September and get to RS before November, you may want to take a peek into this race to see if there is a good car available (or if the opponent present provides a good odds total). It is one of the shortest RS races in the whole year, timewise, so it may be worth doing (depending on the opponent) in order to save as much time as possible in order to reach Top 6 in the rankings. There are several wall-riding and bouncing spots available to make the lap a little easier, but this is a more difficult race than The Sprinters Cup.  
   * Downhill gives much better odds, and we've found that usually an R33 generates as an opponent on the downhill race (which is very much a KotY-winning car). Uphill is usually a lost cause.  
2. **The Sprinters Cup** (November Week 3; Victoria Garden 6 laps; \~6-6.5 minute race): Although this race is slightly longer than the RI races you will be doing, the 500 base Ranking Points from RS combined with the high odds against your starter car could give you a quick route to Rank \#1-6 even for a P2 or P3 finish depending on odds and your current point total, and some good cars can be obtained from this race for finishing in P4 or higher. This race is also easier due to the possible wall-riding one can do to get positions over much tougher AI cars than expected (especially in its **reverse** variant).

## Driving Revolution All C%/S%

A Driving Revolution All C%/S% speedrun’s goal is the most straightforward one of them all: **complete all Driving Revolution missions**. The difference between C% and S% is that the former only requires at least a **passing grade**, while the latter requires the highest achievable rating possible for each stage.

In order to pass a mission with at least a C-rank, **fill the green bar on the bottom-left past the orange indicator line**, and in order to reach an S-rank on a stage, you must:

* **Achieve at least 80% “Perfect” gate passes**.   
* **Have no gates be rated lower than “Great”** (“Good”, “Bad” and “Miss” are an instant restart for S%).  
* **Have no penalties from hitting any walls.**

This is generally a solely skill and knowledge-dependent run, similar to speedrunning mission races in Gran Turismo 4 (or licenses in most Gran Turismo games).   
There are some quirks with speedrunning this category:

* You are **locked to an automatic gearbox**. This may become problematic when driving the faster and more high-powered cars later on, **especially in Level 17**. For cars with traditional manual transmissions, you still have the clutch input available for more control.  
  * The clutch is invaluable for Level 18, as it lets you modulate your speed and rotation in a level where your brakes are always unavailable.  
  * You can hold any of the shift buttons to lock your current gear (the car will not upshift nor downshift while you hold any of the shift buttons).  
* Some stages can be **very** tricky (and possibly inconsistent), especially for S%, by for example forcing you to nearly come to a halt with a very tight window (Level 9-4), purposely go off the track (e.g. Level 7-4 and 9-1), or even lock your gearing, acceleration or braking (e.g. the entire final level).  
* You do not have to hit the gates dead-center to pass through them. You may want to plan a line that lets you hit all the gates with minimal steering inputs.

# Additional information

## Ranking guide

Your total ranking points are determined as the total of the rank points from the **best 9 races from the 12 most recent weeks** in the in-game calendar.

### League Qualification Thresholds

* **RN**: Always unlocked  
* **RIV**: Always unlocked after winning 2 RN races or completing 5 RN races (whichever comes first)  
* **RIII**: Be in the top 800 (\~369 Points)  
* **RII**: Be in the top 500 (\~1155 Points)  
* **RI**: Be in the top 300 (\~2250 Points)  
* **RS**: Be in the top 50 (\~4380 Points), except for King of the Year Grand Prix, which requires being in the Top 6 (\~8850 Points) at the start of December Week 4\.

### Base scores per league

* **RN**: 10  
* **RIV**: 20  
* **RIII**: 50  
* **RII**: 100  
* **RI**: 200  
* **RS**: 500

### Finishing position multipliers

Race Finish Position Points Multipliers for the Six-Car Races.

* **1st**: 1x  
* **2nd**: 0.6x  
* **3rd**: 0.4x  
* **4th**: 0.2x (Placing here or better yields the chance for a prize car)  
* **5th and 6th**: You get no Ranking Points.

Race Finish Position Points Multipliers for the 1 vs. 1 Races

* **1st**: 1x (You must win 1v1s to get the chance for a prize car)  
* **2nd**: You get no Ranking Points.

Your rating points are calculated in the following way:

\[League base points\] × \[odds\] × \[finishing position multiplier\]

So for a race in RII, where you finish a race with 15.0 odds in second place, you will get

100 × 15.0 × 0.6 \= 900 points.

### Routes & RNG Manipulation

There is some amount of RNG manipulation possible to perform to get ideal races more often during a run. Here are some routes that use RNG Manipulation, and some that do not, for reference purposes:

* General Race RNG Manipulation Techniques  
  * In Any%, the frame since boot when you press New Game affects the initial race RNG seed used for the save.  
  * Pressing Join Race can influence the lineup of the next race after Resting. Pressing Join Race, backing out (and doing it one or more times) can potentially generate different lineups. This seems to advance the RNG the same amount of times as backing out of a race preview.  
  * While in the race selection screen, viewing a race's lineup and backing out of the race preview (and doing it one or more times) also seems to advance RNG. This seems to advance the RNG the same amount of times as pressing Join Race alone.  
  * Spending time in the Pre- and Post-Race menus in Enthusia Life (even in **Test Drive** (but not on Airport Square)) advances the RNG. This seems to advance the RNG every 12 frames (0.2 seconds).  
    * This seems to be used for the camera shake present in SOME pre- and post-race screens (e.g. in Dragon Range), but the RNG is run regardless of track, as long as the camera is not attached to the car like a chasecam (e.g. in Airport Square).  
  * Creating particles (tire smoke, sand and dirt clouds, etc.) also increments the race RNG for every single particle that spawns, likely multiple times per particle that spawns.  
* Rank1% \- Estima Route \- NTSC-U \- Nale ([SRDC](https://www.speedrun.com/users/Nale_it) / [RA](https://retroachievements.org/user/elaN))  
  * After finishing the 01/01/W2 race, Rest for two weeks and then press Join Race, you will sometimes see these races:  
    * Rev City (2 laps, 9.9 odds)  
    * Autumn Hill (3 laps, 10.4 odds)  
    * Marco Strada (3 laps, 11.4 odds)  
  * If you do not, Rest for one more week, and you will very likely see a Marco Strada race (3 laps, 10.4 odds).  
* Any% \- Estima Route \- NTSC-U \- Nale ([SRDC](https://www.speedrun.com/users/Nale_it) / [RA](https://retroachievements.org/user/elaN))  
  * After entering the Estima and retiring the 01/01/W1 race, Rest until 01/08/W1, and you will get a Marco Strada race (2 laps, 9.7 odds)  
  * After winning your first RN race, Rest for two weeks, and the next race will contain a Rev City race with double-digit odds. This race will sometimes also let you get a KotY-viable car early. However, sometimes the game will generate a low Rev City (4.6 odds) or Autumn Hill (7.6 odds) race instead.  
  * After winning your second RN race and unlocking RIV, the pathing gets slightly more complicated:  
    * Press Join Race in order to advance the RNG seed, then back out of the race selection and Rest for 3 weeks.  
    * Press Join Race again, then back out of the race selection and Rest for one more week.  
    * If it works out, the game will give you a Rev City (2 laps, 9.1 odds) or Autumn Hill (3 laps, 9.2 odds)  
* Any% \- Beat Route \- NTSC-U \- ImaSiphy ([SRDC](https://www.speedrun.com/users/imasiphy) / [RA](https://retroachievements.org/user/imasiphy))  
  * After entering the Beat and retiring the 01/01/W1 race, Rest until 01/07/W2, and you will get an Autumn Hill (2 laps, 10.6 odds) race.  
  * After completing that race, Rest until 01/08/W4. If you then do not see a race with odds above 8.5, go back and Rest one more week to 01/09/W1, and you should get a high odds Autumn Hill (2 laps, \> 8.7 odds) or Marco Strada (2 laps, 9.1 odds) race.  
  * After completing that race, find a race that will get your ranking points total to above 369 before 01/10/W1.  
  * After that, just hope that good races show up for the rest of the run.  
* Any% \- March / Micra Route \- NTSC-U \- RezaMaulana98 ([SRDC](https://www.speedrun.com/users/RezaMaulana98) / [RA](https://retroachievements.org/user/RezaMaulana98))  
  * Skip to 01/08/W1 \- Mostly you get Rev City 13.8, other times you get a low Autumn Hill that isn't worth doing (and is worth restarting over).  
  * After that race, skip to 01/08/W4, where you will mostly get:  
    * Rev City 13.3, approx. 34% of the time, this is the best outcome  
    * Marco Strada 7.2, approx. 16% of the time  
    * Autumn Hill 8.2, approx. 16% of the time  
    * Marco Strada 4.4, approx. 20% of the time, this is the worst outcome,  and is worth resetting over  
    * Rev City 12.3, approx 14% of the time, this is a good backup choice, though RIII will be far harder to one-shot  
  * After that, you have two options,  
    * Hit Join Race on 01/09/W1, then Rest to 01/09/W4, and you will mostly get:  
      * Rev City 10.1, this is the joint best outcome  
      * Tsukuba Circuit 8.2, this is the worst outcome  
      * Marco Strada 8.5  
      * Marco Strada 10.1, this is the joint best outcome  
    * Skip directly to 01/10/W1, and you will mostly get:  
      * Marco Strada 6.9 (4 Laps), this is the worst outcome  
      * Autumn Hill 12.1, this is the best outcome  
      * Rev City 9.0  
      * Tsukuba Circuit 8.6  
  * After that, skip directly to (or advance to) 01/10/W2, where you will mostly get:  
    * Autumn Mountain 19.6 and Victoria Road 14.7 together. Do Victoria Road 14.7.  
    * Autumn Hill 14.5  
    * Tsukuba 9.5, this is the worst outcome  
    * Marco Strada 11.6  
    * Victoria Road 14.7, on its own  
    * Autumn Mountain 19.6, on its own (**IMPOSSIBLE WITH MICRA**)  
    * Autumn Mountain 19.6 and Autumn Hill 14.5 (**4 Laps**). Do Autumn Hill, as it is one of the shortest available RIII races.  
    * Autumn Mountain 19.6 and Autumn Hill 14.5 (**5 Laps**). Alright, but not as ideal as the 4-lap variant of Autumn Hill.  
    * Victoria Garden 12.2  
    * Autumn Mountain 19.6 and Autumn Hill 19.3 (**BOTH ARE IMPOSSIBLE WITH MICRA**)  
  * After this RIII race, you are free to enter most RII and RI races to get to RS. This is a more beginner-friendly route, though it will not be the fastest route possible.  
* Rank1% \- March / Micra Route \- NTSC-U \- RezaMaulana98 ([SRDC](https://www.speedrun.com/users/RezaMaulana98) / [RA](https://retroachievements.org/user/RezaMaulana98))  
  * Follows the same route as the Rank1% Estima manipulation for RN and RIV, though the races are different.  
  * \[**TODO**: Insert races and odds here.\]  
* Any% \- March / Micra Route \- NTSC-U \- Nale ([SRDC](https://www.speedrun.com/users/Nale_it) / [RA](https://retroachievements.org/user/elaN)) & ImaSiphy ([SRDC](https://www.speedrun.com/users/imasiphy) / [RA](https://retroachievements.org/user/imasiphy))  
  * After entering the Micra and retiring the 01/01/W1 race, Rest until 01/08/W4. If you see an Autumn Hill 11.6 race, enter it and continue.  
  * At 01/09/W1, if you see a Marco Strada 15.5 race, enter it and continue. This is easily the best outcome for this week.+  
    * If you do not, and you instead see a Mystic Caveway 6.1 race, go back, Rest once, and you will hopefully see a Rev City \~10 race. If you do, Rest once more, and you will hopefully see a Marco Strada 14.4 you can do.  
    * If you do not, and you instead see an Autumn Hill 5.7, Rest once, and you will hopefully see an Autumn Hill 6.5. If you do, Rest once more and you will hopefully see a Marco Strada 7.4 you can do.  
      * This is sadly the best race available from getting an Autumn Hill 5.7 race at 01/09/W1.  
    * Other possibilities include:  
      * Mystic Caveway 10.4  
      * Rev City 6.9  
  * If you get the Marco Strada 15.5 race on 01/09/W1, you have two choices:  
    * Skip directly to 01/09/W4 after winning it, You have a chance of getting one of:  
      * Marco Strada 10.1 (3 Laps)  
      * Rev City 10.1 (2 Laps)  
    * Skip directly to 01/10/W1 after winning it. You have a chance of getting one of:  
      * Autumn Hill 12.1, 3 Laps (most ideal)  
      * Marco Strada 10.7, 3 Laps (less ideal)  
  * If you get and do the Marco Strada 10.1 race on 01/09/W4, skip directly to 01/10/W2. You have a chance of getting an Autumn Mountain 14.8 race and a Victoria Road 14.7 race on the same screen / pull. Do the Victoria Road race for a faster race.  
    * You may encounter a 20.3 or a 19.6 odds race on Autumn Mountain. **DO NOT DO THESE RACES, THE OPPONENTS ARE TOO HARD FOR THE MICRA.**  
  * Once you do the Victoria Road 14.7 race on 01/10/W2, you have a chance of getting a Pacific Gateway 17.7 race on 01/10/W3. **THIS IS A VERY HARD RACE, BUT IS WORTH WINNING, AND IT IS POSSIBLE**.  
    * If you need to sacrifice the next week to win this race, do so.  
  * If you do win the Pacific Gateway 17.7 race, you may get a Löwenseering 13.2 race. Do this race.  
    * Races around \~13-15 odds are what you're looking for, aim for Löwenseering or Pacific Gateway.  
  * At this point, you should have enough points before 01/11/W1 to enter RS, so you can enter the Night-Winding Championship, ideally spawning in an R33 GTR as your opponent on the downhill course. Perform all the wall-bounces and drive as dirtily as possible to ensure you win. The game will most of the time give you the R33 GTR, and this will give you enough points for the KotY race.  
    * **THIS RACE IS NOT POSSIBLE IN THE UPHILL CONFIGURATION, YOU WILL LIKELY GET AN ASTON MARTIN VANQUISH AS AN OPPONENT THAT IS TOO FAST.**  
  * On paper, this is the route with the lowest possible floor of possible run time for Any%, but it will be very RNG-dependent.  
* Any% \- Mini Cooper 1275S Route \- NTSC-U \- Nale ([SRDC](https://www.speedrun.com/users/Nale_it) / [RA](https://retroachievements.org/user/elaN)) & ImaSiphy ([SRDC](https://www.speedrun.com/users/imasiphy) / [RA](https://retroachievements.org/user/imasiphy))  
  * Select the Mini. **MAKE SURE TO SELECT GREEN (this is somehow not placebo, no idea why)**.  
  * Skip to 07/W4. If you see an Autumn Hill 20.6 race, do it. **If not, immediately reset**. We're still not sure how to trigger this consistently.  
  * Skip to 09/W2. You will always find a 9+ odds race here. Do that race.  
  * SKip to 09/W4, do an **RN** race that will get you to a total of **369 ranking points or higher**. This skips RIV.  
  * After this, it's all up to the game to give you good races. Ensure to also account for points loss from advancing weeks to make sure you stay on rank 6 or higher for qualification to KotY.  
    * The Mini is capable of going from RI to Top 6 without doing any RS races. In order to achieve this, do an RI race that will raise your points total to 8800 points or more after finishing the race. Try and find RI races that have close to 30 odds.  
      * Try and aim for Victoria Garden 7 or 8 Lap races or Victoria Road (**FORWARD, not reverse**) 4 Lap races, or Pacific Gateway 3 Lap races (harder than the VG/VR races) in RI.  
  * You will likely use one of these cars (should you get them) for KotY:  
    * Audi RS6  
    * Audi A8  
    * Alfa  
* Any% \- Mini Cooper 1275S Route (Safer) \- ImaSiphy ([SRDC](https://www.speedrun.com/users/imasiphy) / [RA](https://retroachievements.org/user/imasiphy))  
  * Select the Mini. **MAKE SURE TO SELECT GREEN (this is somehow not placebo, no idea why)**.  
  * Skip directly to 08/W2, and hope for an Autumn Hill 9.6 race.  
  * After this race, you are on your own. Make sure your two RN races and the one RIV race gives you a total of \~369 ranking points or more.  
  * After this, try and find an RIII race that has \~20+ odds on 10/W1 or later.  
  * After this, try and find an RII race that has \~29+ odds (or some amount of odds that will take you to \~4300 ranking points) **BEFORE** 11/W1.  
  * After this, if you are not in RS, reset the run. Otherwise, do the Night Winding Championship on 11/W1, and hope for it to be a downhill race.  
  * At this point, if you have enough points for KotY qualification and a good car for it, you are set for the rest of the run.  
* Any% \- Mini Cooper 1275S \+ eK Route \- ImaSiphy ([SRDC](https://www.speedrun.com/users/imasiphy) / [RA](https://retroachievements.org/user/imasiphy)), realXP  
  * Select the Mini. **MAKE SURE TO SELECT GREEN (this is somehow not placebo, no idea why)**.  
  * Skip to 07/W4. If you see an Autumn Hill 20.6 race, do it. **If not, immediately reset**. We're still not sure how to trigger this consistently.  
  * Switch to the eK, and find a 17+ odds RN race. Do this race, and you should immediately qualify for RIII (skipping needing to do an RIV race).  
  * Switch back to the Mini, and find a 18+ odds RIII race after **09/04**, that should take you to RII.  
  * Find a 17+ Pacific Gateway for RII, and do that race, which should be enough to take you to RI.  
  * Find an RI race that will take you to a total of \~8700 points (accounting for points losses from skipped weeks), to get you to Top 6\. After that, go to KotY with your KotY car and hopefully win.  
* Any% \- Polo Route (Safer) \- NTSC-U \- ImaSiphy ([SRDC](https://www.speedrun.com/users/imasiphy) / [RA](https://retroachievements.org/user/imasiphy))  
  * Skip directly to 08/W3, and hope for Autumn Hill 12.0.  
  * Do any 2nd RN and RIV race that would take you to at least 369 ranking points after the RIV race.  
  * Do a 17+ odds RIII race.  
  * Do an RII race that'd take your ranking point count to at least 4380\.  
  * Do Night Winding Championship on 11/W1. Hope for the R33 to appear as an opponent in this race. This should qualify you to King of the Year.  
* Any% \- Polo Route (Riskier) \- NTSC-U \- Azullia ([SRDC](https://www.speedrun.com/users/Azullia) / [RA](https://retroachievements.org/user/Azullia)), realXP, Nale ([SRDC](https://www.speedrun.com/users/Nale_it) / [RA](https://retroachievements.org/user/elaN)) & ImaSiphy ([SRDC](https://www.speedrun.com/users/imasiphy) / [RA](https://retroachievements.org/user/imasiphy))  
  * Skip directly to 08/W3, and hope for Autumn Hill 12.0.  
  * From here, you have two choices:  
    * Skip directly to 09/W2:  
      * If you see Autumn Hill 11.3, hit Join Race 3 more times, then skip to 09/W3 and hope for a Marco Strada 17.5.  
      * If you see anything else, restart.  
    * Skip directly to 09/W3 and PRAY you get Marco Strada 17.5.  
  * Hit Join Race once in 09/W4, then skip to 10/W1, to hopefully get Autumn Hill 19.3 (RIV).  
  * On 10/W2, hopefully find a Victoria Road 21.6 (RIII).  
  * On 10/W3, hopefully for a Pacific Gateway 27.8 (RII).  
  * Hit Join Race six times on 10/W4, then go straight to Night Winding Championship (11/W1). Winning this should qualify you to KotY.  
    * You should be going up against the R33, but this race is still very hard without a maxed out Polo.

These RNG manipulation paths usually work, but sometimes the game's RNG will not cooperate and give you different races than expected.

## Other Resources

* [Enthusia Tuning guide (Google Docs)](https://docs.google.com/document/d/1uSfw1M4AwCnkd-VSn6UspavUmq7nnC9nlKiVj8PUKOA/view)  
* [Enthusia Tuning guide (GitHub)](tuning_guide.md)  
* [Rank1% Premade Save w/ Honda Beat](https://www.speedrun.com/epr/resources/mv3jw)  
* [GT Planet Enthusia FAQ](https://www.gtplanet.net/forum/threads/enthusia-f-a-q-everything-you-need-to-know.77460/)  
* [Paul Schuurmans' Enthusia: Professional Racing Page](https://home.hccnet.nl/paul.schuurmans/gt/epr.html)  
* [WikiHouse Page on Enthusia (Japanese)](https://www.wikihouse.com/enth/index.php?ENTHUSIA%20Wiki)