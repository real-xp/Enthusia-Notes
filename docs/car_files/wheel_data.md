# Wheel disc diagnosis

> AI Generated and Researched, beware.

Evidence uses `SLUS_209.67` through idalib and car `00000003.car`. This note distinguishes model surfaces from runtime drawing passes; it does not attempt a full PS2 shader recreation.

## Opaque wheel cover is a mask pass

The front wheel includes a six-triangle outer octagon, material 29, outside the actual rim. Its flags are `0x0003000000019F12`: render mode `(flags >> 8) & 15` is `0xF`; flag `0x10000` is set. Although its source material names texture 3, the game does not use that descriptor for this pass.

`sub_262AF0`, instructions `0x262E60` through `0x262E7C`, handles mode `0xF` by setting runtime material type five and clearing both TEX0 and TEX2. The debug string at `0x48BB98` explicitly calls this a mask. Its own low-TBP `0xFFFF` texture is therefore not a missing ordinary bitmap requiring a white fallback.

`sub_261E88`, instructions `0x26224C` through `0x26228C`, handles runtime type five. When `flags & 0x10000` is nonzero and renderer byte `+0x190` is zero, it branches to the next material alternative without submitting this pass. Renderer initialization `sub_261310` writes that byte to zero at `0x26140C`. When enabled, this pass explicitly clears texture registers again; it is not an ordinary opaque surface even in that state.

Therefore a neutral static import should omit this conditional mask from visible mesh geometry, or retain it separately hidden. Keeping its six triangles as an opaque neutral material physically covers the modeled rim. Actual rim batches reference texture 41 and remain behind it. This is an executable-supported correction, independent of guessed tire colors or external texture assets.

## PSMT4 palette address correction

Current PCSX2 source provides a precise CSM1 rule: PSMT4 palette loading reads the block at CBP and writes sixteen colors into the internal CLUT cache slot selected by CSA. CSA changes the cache destination, not the source VRAM address. `WriteCLUT32_I4_CSM1` calls `WriteCLUT_T32_I4_CSM1(BlockPtr32(0, 0, CBP, 1), clut + ((CSA & 15) << 4))`. The lower-level loader reads that block's first sixteen colors. Source: [PCSX2 GSClut.cpp](https://github.com/PCSX2/pcsx2/blob/master/pcsx2/GS/GSClut.cpp), functions `WriteCLUT32_I4_CSM1` and `WriteCLUT_T32_I4_CSM1`.

That direct sixteen-color-load rule does **not** describe Enthusia's actual sequence. `sub_261E88` writes material TEX2 first (packet `+0x40`) and TEX0 second (packet `+0x50`), at instructions `0x2623C8` and `0x2623EC`. Texture 41's TEX2 has PSM `0x13` (PSMT8), CBP 44, CSA zero and CLD one. It loads a full 256-color bank. Its following TEX0 has PSM `0x14` (PSMT4), CSA four and CLD zero: it selects cached colors 64..79 without reloading palette memory. TEX0 also retains its own texture dimensions and TBP.

The decoder must combine **palette CBP from TEX2 with palette CSA from TEX0**. Source palette entry is `index + TEX0.CSA*16`, interpreted using the 256-color load's source permutation. Taking both CBP and CSA from TEX2 incorrectly reads bank entries 0..15, which happen to be black alpha data. The existing swap of index bits three/four plus CT32 pixel addressing matches all first 128 entries of PCSX2's `clutTableT32I8`, checked by an assertion-based experiment.

With the correct bank offset, sample texture 41 recovers the actual BBS-style multi-spoke rim bitmap: transparent gaps and opaque gray spokes, including gray values 53, 58, 64, 70, 76 and 83. It is **not** an alpha-only material requiring an invented grayscale reconstruction. Sample texture 8 also recovers body grayscale detail, and texture 16 recovers red RGB `(201, 26, 27)`. `.scratchpad/wheel_clut_check.py` isolated the palette-read change before importer edits; its scratch images are disposable, not deliverables.

## Surface alpha and stationary wheels

Material byte `+4` selects a runtime primitive-alpha group. It is not itself a direct transparency flag. `sub_261E88` reads the corresponding alpha from renderer `+436+group` (`0x262144` / `0x2621D0`) and omits a pass when that value is below four (`0x2621F0`). `sub_193B48` selects group-zero alpha 128 and group-one alpha zero for a stationary/slow wheel, reversing those values when its wheel-speed state reaches 400. Sample rim materials 62 and 71 use the same texture/flags but groups zero and one respectively. A neutral stationary import should keep group zero and omit group one on wheel parts; importing both creates overlapping surfaces that the game does not draw simultaneously.

GS TEST is the useful coverage metadata: it resides at material `+0x28`. Sample rim value `0x5060B` enables alpha test, selects GEQUAL with reference 96, and keeps failed pixels unchanged. Real rim RGBA therefore supplies opaque spokes and transparent gaps through alpha testing. Normal body material 18 and shared tire materials have alpha testing disabled (`0x50000`), so their texture alpha must not automatically become Blender surface transparency. Those values can control paint/layer blending while the complete surface remains covered. Keeping these ordinary surfaces opaque while approximating their color layers is preferable to making the roof or tire translucent.

### NISMO 400R additional darkening layer

Car `00000160.car` exposes a second layered case. Wheel part one includes normal rim material 107, texture 37, stationary alpha group zero; matching high-speed material 108 uses texture 39 and group one. Material 107's 46 triangles include a center polygon fan at local X approximately `0.0945`, so its real RGB/alpha bitmap already defines the hub and spokes. Vertex radii alone misleadingly suggest an annulus: triangles connecting the inner contour fill its center.

A separate two-triangle planar quad lies in front at X approximately `0.10089`. Its material 46 uses texture 38, which has black RGB throughout and a wheel-shaped alpha pattern. TEST is `0x5160B`: GEQUAL reference 96, but failed pixels write framebuffer only (`AFAIL=1`) rather than KEEP. Treating this black drawing layer as an ordinary opaque/cutout Blender surface covers the genuine rim fan.

A narrow static approximation can omit such additional wheel layers when all RGB is zero, geometry is a two-triangle quad, and TEST uses framebuffer-only alpha failure. Retain parsed geometry in format results; omit only this specialized visible Blender layer. A real RGB rim must remain behind it. This preserves the original normal rim image without inventing spokes or colors. The difference between source flags low bytes `0x10` and `0x12` was not traced to a definitive GS state bit; do not claim that bit alone identifies the effect.

## Scope and limits

### Shared tire textures recovered

The original shared tire bitmaps are present in the North American game's archives. Executable `sub_270520` registers `pvram_000_a.bin` and `pvram_001_a.bin` at file-class four indices zero/one. `sub_142C00` loads the first bank into the pool returned by `sub_142BF0`; `sub_143048` loads the second into the pool returned by `sub_142E60`.

In the supplied US `D000.BIN`/`D001.BIN`, sector-index entries 103 and 104 identify these banks. First bank byte range is `[0x4248C800, 0x424D7800)`, 307,200 bytes. Its magic is ASCII `ABCD`; header u32 `+8` points to main upload DMA (`0xA00`), `+0xC` points to palette DMA (`0x46A70`), and `+0x10` counts 79 descriptors. The second bank starts `0x424D7800` and has ten descriptors. Adjacent entries 100..102 are distinct `_e.bin` metadata, explaining why using those extracted files as texture banks failed.

`sub_261310` resolves shared material indices through executable table `qword_383938`: indices 1..7 select first-bank textures 43..49; index eight selects texture 36; index nine selects texture 35. Sample wheel material 20 uses index one / texture 43, a tire sidewall; material 19 uses index two / texture 44, tread. Thus these indices refer to shared surface textures, not exclusively generic environment maps.

ABCD descriptors start `+0x20`, stride `0x20`: u16 X/Y at `+0/+2`, width/height at `+4/+6`, flags at `+0xC`, TEX0 at `+0x10`, TEX2 at `+0x18`. Textures 43..49 are atlas crops with flag one. Their TEX0 PSM is `0x2C` (PSMT4HH), selecting the high nibble of the CT32 pixel's highest byte. The atlas upload is PSMT8H (`0x1B`), logical 640 by 448, one byte per pixel. Its GIF IMAGE payload begins main-DMA offset plus 112. Consequently direct crop decode can take `payload[(y+Y)*640 + x+X] >> 4` as palette index without emulating unused low CT32 channels.

Palette DMA uploads CT32, 64 by 64, at BP 16320. Source palette CBP is raw TEX2 CBP plus 16320; TEX0 CSA chooses the cached palette slice, as for local car textures. Reuse CT32 addressing and the PSMT8 CLUT permutation. Descriptor 43 crops `(0,0,128,128)`, 44 crops `(128,0,64,64)`, and 45..49 crop X 192, 256, 320, 384 and 448 respectively, Y zero, size 64 square.

Runtime bank initialization `sub_142D90` sets first-pool type one. `sub_143590` relocates main upload DBP from raw 6720 to 7040 and adds 7040 to descriptor TBP; it adds 16320 to palette CBP. An offline decoder can keep raw atlas DBP 6720 and apply the same base to descriptor TBP, or decode logical atlas crops directly. Applying runtime base to only one side would be wrong.

Independent scratch decoding recovered tire 43 RGB values zero through 37 and tire 44 RGB values two through 33, with recognizable dark sidewall/tread patterns. These are game-provided rubber colors, not guessed neutral materials. Alpha remains a separate PS2 blend control; blindly using it as Blender transparency makes the tire surface excessively transparent. Opaque RGB sampling for shared tire surfaces is an explicit static material approximation until the complete draw layering is reproduced.

Exact runtime mask enable state, destination-alpha operations, reflections, wheel-motion blending and game lighting are not reconstructed here. Static import should preserve tire/rim mesh surfaces and original bitmap color while keeping specialized masks out of its opaque surface set.
