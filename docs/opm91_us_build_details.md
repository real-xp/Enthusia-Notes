# Official Playstation Magazine Demo Disk 91 (US)
## Overview
`SCUS-97443`

In the `Official U.S. PlayStation Magazine Demo Disc 91`, there is a demo of Enthusia.

This also seems to be from `April 2004`

A showcase can be seen here - https://www.youtube.com/watch?v=LKWDqTi6pDo

The showcase is vanilla showcase without any patches applied.

A much more detailed inspection in this build can be found on [opm91_us_build_details.md](opm91_us_build_details.md)

Some features include
- Three tracks
  - Speediapolis Ring
    - Cars are
      - Audi R8 Team Goh 2004
      - Toyota GT-ONE
      - Nissan R390 GT1
      - Mazda 787B
  - Burgenschlucht (Game refers to banners inside map as Werz Bahn)
    - Cars are
      - Volkswagen Golf R32
        - The logo for the manufacturer looks different from final game
      - Audi TT Quattro Coupe
      - Alfa 147 GTA
      - Aston Martin V12 Vanquish
      - Mercedes Benz CLK-DTM
  - Marco Strada Wet
    - Cars are
      - Fiat 500F
        - This uses a different default color
      - smart fortwo cabrio
      - Mini Cooper S
      - Citroen Saxo VTS
      - Mazda Roadster RS-II
      - Lotus Elise 111S
- The tracks selection preview are actual videos and not just static images
- A crosshair like thing appears when selecting or hovering over items
- Apart from the usual modes, there is a "P" mode on top right 
  - E - Enthusia Life
  - P - ? (Presumed Professional Racing) (later found out as Prize Racing)
  - F - Free Racing
  - T - Time Trial
  - 2 - 2P mode
  - Dr - Driving Revolution
  - Da - Data
  - O - Options
- The UI looks very different
- The car selection screen shows difficulty below the car preview
- Lighting is different in garage
- The car selection camera is very different 
- Race cars use the same Analogue RPM gauge that normal cars use, rather than their own custom gauge
- VGS is very different 
  - The red needle represents the tire direction it seems
  - The arrows represent the G loads
  - Its harder to read in general
- Rear view camera has a much much lower render distance than final game 
- 787B sounds much different than final game
- Speediapolis music is very different compared to final game
- Burgenschlucht uses same music as Speediapolis
- Music in Marco Strada Wet is different than final game
- A pit stop sign was removed from Marco Strada wet, which means there was meant to be pit stops.
- Result screen is much much different than final game
- When loading the game, the control scheme shows right stick to "Move camera" which is not present in final game or even demo build
- Minimap looks much much smaller

## Internal Strings
- In aspiration, they called `Turbocharged` `TB` in short form, which, uh, could be misinterpreted as `Tuberculosis`. No wonder they changed it in final game. Call it `Turboculosis`.
- There does not seem to be a `F` class. Only `R, A, B, C, D, E` classes.
- `Boxer` engine is called `Flat` engine.

From the OPM Demo of Enthusia, some more removed cars were found
- `フェラーリ Ｆ４０` = `Ferrari F40`
- `フェラーリ ３６０モデナ` = `Ferrari 360 modena`
- `ベントレー コンチネンタルＧＴ` = `BENTLEY Continental GT`
- `BUGATTI ＥＢ１１０ GT` = `BUGATTI EB110 GT` (technically is there as a Dauer)

Also in terms of brand
- `アキュラ` = `Acura`
- `いすゞ` = `ISUZU`
- `フェラーリ` = `Ferrari`
- `ブガッティ` = `Bugatti` (basically Dauer in final game)
- `ベントレー` = `Bentley`
- `マクラーレン` = `Mclaren` (we believe they mean the `BMW Mclaren F1 Race Car`, which in final game is referred by `BMW`)
- `マセラッティ` = `Maserati` (probably depreciated faster than the game died)
- `ポルシェ` = `Porsche`

## Removed Features
Read [facts_and_glitches.md](./facts_and_glitches.md#removed-prize-race-mode) for more information.

## Main Menu Code
The game by default goes to `FREE RACE` mode. This is because the menu id is hardcoded.

This can be changed by using some patches in PCSX2.

At `0017A774`, `addiu s0,zero,0x9` is the instruction set. The `0x9` value at the end is the actual menu ID. Below is the table for different menu IDs.

| ID                | Name  | What It Does                                        |
| ----------------- | ----- | --------------------------------------------------- |
| `0x0` `0x1` `0x2` | ??    | Returns Back To Main Menu                           |
| `0x3`             | ??    | Shows the intro movie                               |
| `0x4` `0x5`       | ??    | Returns back to Main Menu                           |
| `0x6`             | ??    | Black Void                                          |
| `0x7`             | DR    | Opens Driving Revolution (barely works)             |
| `0x8`             | TA    | Opens Time Trial (Broken without patches)           |
| `0x9`             | FR    | Opens Free Race                                     |
| `0xA`             | PR    | Opens Prize Race (Broken without patches)           |
| `0xB`             | EL    | Opens Enthusia Life (Broken without patches)        |
| `0xC`             | Da    | Opens Data / Debug Menu                             |
| `0xD`             | 2P    | Opens 2 Player Mode                                 |
| `0xE`             | 2P    | Opens 2 Player Mode, auto picks Audi R8 2004 LeMans |
| `0xF`             | OP    | Opens Options Menu                                  |
| `0x10`            | Test1 | Single Player test, 1v1, Random Map + Random Car    |
| `0x11`            | Test2 | 2P test, 1v1, Random Map + Car                      |

## Prize Mode
The game preserves the PR mode code. It is just mainly inaccessible in the demo. This can be accessed by some patches.

### Details
By default, you get 100 chips as your starting money.
For the first level, your bet is 10 chips which is multiplied by your odds to win against other 5 opponents. The chips you get depend on what position you get.

Every time you get 3rd or higher, you got `n` number of your betted chips and the round would count up.

If you lose all your chips, ie, go equal or under zero, you lose.

The winning criteria is probably winning all the rounds.

This basically worked like a roguelike mode.


### Technical / How To Use
To note - The allocation of memory in PR is broken. This is fixed by patches.

> `sub_17DBB0` starts a task by calling vtable entry 2 for a heap spec and creating a heap group.
PR's spec (`sub_169FB8` -> `0x32E2D0`) is `{0x300000, 0x600, 0x1B00000, 0x80}` = 3 MB + 27 MB.
The demo's parent pool is only 20.75 MB (FR asks 0.5 MB + 19 MB). The 27 MB allocation in
`sub_166CF0` returns NULL (`0x166F20`), the allocator writes through the null block
(TLB Miss stores at 0x167054 / 0x1666F4 / 0x16711C), and `sub_1671D0` later spins on the
corrupted free list at `0x1671F8`: 0 FPS, kernel PC 0x8000FDBC on real hardware Fix: shrink the spec to 1 MB + 17.5 MB (`0x32E2D0 = 0x100000`, `0x32E2D8 = 0x1180000`).

The patch for a "***working***" copy of PR is in the `.pnach` file linked [here](../assets/patches/opm_91/SCUS-97443_D2FCA12E_PR.pnach).

You can increase the starting bets and other stuff using the other patch [here](..//assets/patches/opm_91/SCUS-97443_D2FCA12E_PR_OPTIONAL.pnach)

Note - This gamemode is really really really unstable.

## Enthusia Life
The game preserves the EL mode code. It is just mainly inaccessible in the demo. This can be accessed by some patches.

### Details
EL mode worked similar to the EL in final game, except there are a few notable differences.
- The points and skill points are different from each other
- You won cars by raising your skill level
- There was a qualify option in races to qualify for higher starting position, basically a time trial mode.
- Screens looked quite different.
- Icons for tuning were different.
- Race menus looked different.

### Technical / How To Use
Following the same strategy as PR mode. It is the same case.
In this case, even after fixing the HEAP, you need to fix the broken races. This can be done by other set of patches.

The patch for a "***working***" copy of EL is in the `.pnach` file linked [here](../assets/patches/opm_91/SCUS-97443_D2FCA12E_EL.pnach).


