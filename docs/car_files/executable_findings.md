# Enthusia executable evidence

> AI Generated and Researched, beware.

Later wheel-specific findings refine the shared-surface and palette interpretation
below; see [wheel_materials.md](wheel_data.md) for recovered tyre maps, the
TEX2/TEX0 cache sequence, static alpha groups and visible-pass corrections.

Analysis target: North American `SLUS_209.67`, existing IDA database copied into workspace scratchpad. Addresses below are executable virtual addresses. Source binary/database left untouched. Results obtained through idalib disassembly and Hex-Rays; MIPS/VU inline assembly checked where decompiler omitted data movement. These are reverse-engineered observations, not official format specifications.

## Package and PC container

`sub_27E848` validates archive headers: little-endian u16 at `+0` equals `0xFC87`, bytes `+2` and `+4` zero, byte `+3` alignment (car caller passes 16), signed count at `+8`, entries begin `+0xC`, stride eight. Entry fields are offset and size. `sub_27E950` returns archive base plus entry offset only when entry size is positive. `sub_2711F8` formats car filenames as `%08d.car`; `sub_272D40` registers `pack/carpk`.

`sub_262AF0` is car-model initialization. It tests PC u16 magic `0x4350` (`PC`) and relocates four nonzero u32 fields at PC offsets `+4` through `+0x10` relative to PC base. PC `+4` points to CA geometry, `+8` to GS textures. It requires geometry u32 magic `909132099` (`CA06`).

## CA06 tables and packets

`sub_262F90` initializes CA06: u16 `+0x10` node count; node table starts `+0x20`, stride `0x100`; u32 `+0x1C` material-table offset relocates relative to CA base. Node u32 `+0` geometry pointer relocates relative to CA; u16 `+0xC` must be nonzero for valid geometry. Node `+0x80` and `+0x90` contain bounding-box min/max; initializer writes midpoint at `+0xA0`. Node `+0x18` becomes runtime index. Header `+7` becomes runtime marker `0x56` after relocation.

`sub_262AF0` reads u16 CA `+0x12` material count, CA `+0x1C` material table, and accesses materials with `0x40` stride. Material signed u16 `+2` selects GS texture descriptor. Material u64 `+0x10` receives TEX0, `+0x18` receives TEX2. Material flags at `+0x38` select environment/reflection/mask and other specialized passes; these effects require more than a diffuse Blender material.

`sub_261E88` consumes geometry records: u16 packet `+0` is payload quadword count; signed byte `+2` equal `-1` terminates; signed byte `+3` counts material alternatives. Material indices begin as signed u16 at packet `+4`. Payload begins packet `+0x20`; next record is `packet + 0x20 + qwc*16`. Thus packet `+4` is a material index, not a texture index. Multiple materials can refer to the same geometry payload.

## LOD and transforms

`sub_261E88` loops part indices 0 through 14. Selected node is `part + 16*(selectionByte-1)`; zero selection hides a part. This verifies LOD groups of sixteen node slots. `sub_262AF0` initializes selection bytes to one, selecting first LOD group. Node 15 is drawn separately by `sub_2626A0`; it belongs to a special mask/shadow-like pass, not ordinary body geometry. Semantic interpretation of that pass is provisional.

Fallback part matrices come from node `+0x40`; three basis vectors occupy the first three 16-byte rows, translation occupies node `+0x70`. Stored fourth components are not a conventional homogeneous 4x4 matrix: ignore those components when constructing Blender affine transforms. `sub_261E88` multiplies root and node matrix for parts configured to use fallback transforms. Part zero defaults to root transform.

Actual car animation overrides parts 0 through 8. `sub_193B48` assigns wheel-state matrices 2..5 to rendered parts 1..4 and brake-state matrices 6..9 to parts 5..8 through `sub_261658`. That setter copies a supplied matrix and disables fallback node-matrix multiplication for that part. `sub_1927E0` obtains wheel transforms from physics-object `+5376+96*i` and brake transforms from `+4992+96*i`; `sub_281CC8` returns each transform's matrix at `+32`.

Static wheel centers are explicit in `sub_1092D8`, instructions `0x109534` through `0x109644`. Physics parameters begin at physics-object `+528`; `sub_188750` copies 488 bytes of these parameters. Parameter offsets and center formula:

| Parameter | Offset |
| --- | --- |
| Wheelbase | `0x10` |
| Front/rear track | `0x14`, `0x18` |
| Front tire width/aspect/rim inches | `0x3C`, `0x40`, `0x44` |
| Rear tire width/aspect/rim inches | `0x48`, `0x4C`, `0x50` |

Front centers: `(+/-frontTrack/2, frontWidth*frontAspect + frontRim*0.0254/2, wheelbase)`. Rear centers: `(+/-rearTrack/2, rearWidth*rearAspect + rearRim*0.0254/2, 0)`. Parts 1/5 front positive-X, 2/6 front negative-X, 3/7 rear positive-X, 4/8 rear negative-X. Runtime replaces the entire wheel matrix, so static import uses those centers with identity rotation: right-wheel vertices already contain mirroring. The main body likewise overrides its CA fallback at all LODs. Suspension, steering, rotation, camber and running-game deformation require simulation state. See [placement and material evidence](glass_paint.md).

## GS02 and texture base correction

Car-specific texture initializer is `sub_262A30`. GS u16 `+4` is descriptor count; signed u16 `+6` is texture-base adjustment. Descriptors begin GS `+0x20`, stride `0x30`. Descriptor `+0x10` is TEX0 and `+0x20` is TEX2 (not TEX1). Material init reads these at `GS+48*textureIndex+48` and `GS+48*textureIndex+64`.

For descriptor byte `+0xF == 0`, initializer adds signed GS `+6` to TEX0 and sets bit 62 in TEX2. This adds the GS base to TBP0 while preserving palette CBP. In sample `00000003.car`, GS `+6 == 64`: raw TBP0 zero therefore means byte address `64*256 == 0x4000`, avoiding false overlap with CLUT at CBP zero. For descriptor byte `+0xF != 0`, initializer adds GS `+6` plus u16 GS `+0xC` and bit 62 to TEX0, then clears TEX2. The alternate branch appears to support special external/dynamic textures; name remains provisional.

`sub_261E88` sends material `+0x10` to GS register `0x06` (TEX0_1), material `+0x18` to `0x16` (TEX2_1). TEX2 therefore supplies the effective palette override when nonzero. GS `+0x10` is upload DMA pointer, `+0x1C` further texture upload/palette data; both relocate relative to GS base. `sub_262A30` also writes DMA data pointer at upload `+4` and zeroes upload `+0x24` before submission.

Correction from subsequent wheel investigation: register ordering matters. The draw packet writes TEX2 **before** TEX0. For PSMT4 car textures, TEX2 preloads a 256-color palette bank, then TEX0 selects its sixteen-color CSA slice without reloading. Decode CBP from TEX2 and CSA from TEX0, not both fields from TEX2. See [wheel material evidence](wheel_data.md). Reading the wrong CSA slice falsely made genuine rim/body textures appear to be black alpha-only masks.

### Absent TBP and shared reflection passes

Raw descriptor TBP `0xFFFF` must not be decoded as ordinary car texture pixels. In the inspected car, those descriptors participate in materials whose texture registers are replaced by shared environment/effect registers. `sub_262AF0` checks material byte `+0x3B` low nibble for ordinary render modes: a nonzero value selects a shared register pair from `qword_383938[2*n]` and `[2*n+1]`, overwrites material TEX0/TEX2, and sets runtime material type six. Example: material 19 references missing car texture 49, but shared index two replaces its texture; material 20 references missing texture 40, but shared index one replaces its texture. Thus substituting either missing texture as a white wheel diffuse map produces an incorrect opaque disk.

Material mode `(flags >> 8) & 0xF` also matters. Modes `0xA` and `0xB` initialize runtime types one and three, whose draw paths use shared environment maps instead of the car descriptor. Mode `0xD` uses fixed register values; mode `0xF` is a mask-related pass. Shared pairs initialize in `sub_261310`, using global texture pools returned by `sub_142BF0` / `sub_142E60`. Their files use a separate texture-bank format with 32-byte descriptor stride, unlike GS02's 48-byte stride. Subsequent investigation established filenames `pvram_000_a.bin` and `pvram_001_a.bin`, recovered the banks from the US archives, and decoded original tire bitmaps; see [wheel material evidence](wheel_data.md). Shared indices 1..7 are tire surface textures, so calling every shared pair an environment map was too broad.

Descriptor u16 `+6` is not a general index into the same descriptor array: sample values increase from zero to `0x2B1` while the array has only 54 elements. Its meaning remains unknown; treating values 15 or 32 as descriptor references merely because they happen to be in range is unsupported.

For a static Blender approximation, retain ordinary diffuse passes and subsequent unconditional source-alpha detail passes. Ordered ordinary passes can contain door handles, window attenuation and defroster artwork; selecting only the first loses authored detail. Environment passes remain approximate PBR reflection layers. Missing shared references must not become opaque white disks. Alpha-only black maps require their blend context. See [ordered-pass evidence](glass_paint.md).

## Remaining unknowns

VU index flags beyond observed triangle suppression were not fully decoded. No claim that `0x4000` alone determines winding. Subsequent work recovered factory colours, baked W brightness, outward normal conversion and stored rigid animation; see [paint/glass evidence](glass_paint.md) and [auxiliary data](auxiliary_data.md). Exact environment/destination-alpha effects and procedural vehicle motion remain incomplete. No in-game debugger capture performed.
