# RNG
Usually referred to as `Random Number Generator`, this game is filled with a lot of it.

**Please note that everything here is incomplete and is still being worked on**

This game has two RNG systems
- [MT](#mt)
- [LCG](#lcg)

# MT
This is called `Mersenne Twister`. Read more at [Mersenne Twister Wikipedia](https://en.wikipedia.org/wiki/Mersenne_Twister)

This game particularly uses the standard `MT19937` implementation of the algorithm.

## Game Usage
This is mainly used for generation of car prize after a race is won.

## Seed
There is no single `seed` for MT. It uses a state list which is set at set points in the game (meaning reseeded), but for generation of car prize, it just iterates through the state list.

Default fallback seed of the game is `5489`, which is not used anywhere ingame.

The places where the seed is changed (reseeding) is -
- Start of the game 
  - Before KONAMI LOGO
- Before Pressing `Start` before main title screen 
  - at the place where `ENTHUSIA` sound plays
  - This is very interesting as the seed depends on the number of frames that have passed since start of game. So, in a way, RNG seeding here is frame dependent.

# LCG
This is called `Linear Congruential Generator`. Read more at [Linear Congruential Generator Wikipedia](https://en.wikipedia.org/wiki/Linear_congruential_generator)

This game uses a 64 bit implementation of `LCG`

## Game Usage
It is used in a lot of scenarios like
- Particle Generation
- Race Generation
- Camera Shake
- Other places

## Seed
The way the game implements LCG is very weird. **IT STORES A GLOBAL SEED THAT IS CHANGED BY A LOT OF FUNCTIONS, FOR A LOT OF DIFFERENT PURPOSES, EVERY FEW FRAMES, (WHICH WILL BE LISTED BELOW). This is SIMILAR or rather basically the SAME as games like SUPER MARIO 64**

The game reseeds at multiple places, sometimes just changing the seed, and sometimes also processing the RNG by running the LCG function. These places are (incomplete list below) -
- Start of the game 
  - Before KONAMI LOGO
- Before Pressing `Start` before main title screen 
  - at the place where `ENTHUSIA` sound plays
  - This is very interesting as the seed depends on the number of frames that have passed since start of game. So, in a way, RNG seeding here is frame dependent. Although here unlike `MT`, this is not particularly useful.
  - looks like the seed is equal to number of frames passed
- Generation of races 
  - So when resting, pressing `New Game` or `Continue` or progressing in a week
- Particle Generation 
  - So in Rain maps, Tire Smoke, Snow, etc.
- Camera Shake 
  - In the Pre and Post race view, there is a subtle camera shake for some races
  - This is most noticeable in maps like Dragon Range and Wild West Enduro
  - Any map with cinematic camera will have camera shake, and will change the RNG
    - So this means maps like Airport Square with a static camera behind car back wont change the RNG

Common Misconseptions about RNG seeding -
- RNG is changed when choosing car paint
  - This is completely **false** as from our finding, car paint does NOT affect the RNG at all.

The game seed is (from our findings) stored at the memory address `004300E8`

The seed is kind of stored as a state in save file near `0002E960` (Viewable in `HxD`). This changes if you progress in the game and save (rest, finish race, etc).

# RNG Manipulation
A detailed RNG Manipulation is in [this document](rng_manipulation.md)


