# RNG Manipulation
RNG Manipulation in this game might as well be the worst thing to exist. It is as bad as Super Mario 64.

One of the better documents for this is [Enthusia Speedrunning Guide](https://docs.google.com/document/d/1jX8PJZ2lZpuxxPWasJbqe8-HDl9FXBfmXOCUV5JHRO0/edit?tab=t.0#heading=h.7rmb92kldexo)

There are two kind of RNGs you can manipulate as discussed before in [rng.md](rng.md) - MT and LCG. MT is for Car Prize, and LCG is for Race Lineups and Odds

## MT Manipulation
Discussing MT first, as it is the easiest - There is really only one way to manipulate the Car Prize RNG as of current
- When you load in the game and are at the "ENTHUSIA Press Start Button" screen, when you press the button and the screen is transitioning, just **after** ENTHUSIA sound, the RNG rerolls.
- This RNG reroll is based on the number of frames spent since the start of the game.
  - For example, if you spent 820 frames since start of the game and pressed the button at the 820th frame, the MT will take in the seed with the `820`, and run it through the algorithm
  - So this means, the MT gets reseeded
- So when you load in the game and get a new car prize, it will be different

## LCG Manipulation
LCG Manipulation for Car Lineup and Odds is much more difficult than MT one

There are a lot of ways to manipulate this for change of car lineup and odds next week

Lets take an example of Week 8 Year 1

### Method 1 (JOIN RACE BUTTON)
- When you are in the EL screen and want to manipulate RNG for the next week
- Press `JOIN RACE` button
- Pressing this progresses the RNG by 1 step
- Now you can go back, rest a week, and you will have a different lineup as compared to if you didnt press `JOIN RACE` and rested
- You can press `JOIN RACE` as many times as you want, and the RNG will be progressed by a step

### Method 2 (JOIN RACE -> CLICK RACE)
- Same as the previous method, but instead of going back from `JOIN RACE` menu, you click in the race (so it shows you the opponents of that race)
- Going in the race (not actually joining the race, just seeing the opponents) also progresses the RNG by one
- Now you can go back to `JOIN RACE` screen (the screen with events with their odds)
  
Using this, the RNG will be progressed. In a diagram way, it can be described as

`JOIN RACE -> GOING BACK` = 1 step

`JOIN RACE -> CLICK ON RACE -> GO BACK -> GO BACK` = 2 steps

`JOIN RACE -> CLICK ON RACE -> GO BACK -> CLICK ON RACE -> GO BACK -> GO BACK` = 3 steps

### Method 3 (PARTICLE GENERATION)
For some reason, particle generation can also result in RNG changing for the next week. To do this
- Just do a race (in EL ofcourse) and generates particles like tire smoke or rain or stuff
- You can quit the event, or finish it, but the particles will progress the RNG
- Next week you will see different lineups
- This is not the most ideal way, but it is nice to be aware about this

### Method 4 (CAMERA SHAKE / PRE AND POST RACE SCREEN)
For some reason, the cinematic camera preview just before starting a race and after finishing a race has a subtle camera shake. This camera shake results in RNG being progressed. This happens once every 12 frame. Some steps to recreate this are -
- Join a race with cinematic camera
- You can either join the race, or retire. If you are quick enough, with perfect inputs, you can manipulate this RNG.
- One of the best examples is Week 1 Mystic caveway with Mini. Retiring as quick as `ImaSiphy` with insanely quick inputs results in a pretty consistent next week lineup.