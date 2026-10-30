# `.car` File

Huge thanks to `King Jack`, `Xan` and `Bogi76` for help with this!

These hold the actual car models and data about them. They can be found in `\pack\carpk\` as `.car` files.

# Header
It is a typical FC87 header. Read more about that in [here.](fc87_file.md)

The cars contain 6 entries in total.
1. Model + Texture Container
2. Unknown
3. Unknown
4. Unknown
5. Vehicle Physics + Dimensions
6. Color Data


# Model + Texture Container (`PC`)
PC prefix is `50 43 00 02`. PC-relative u32 `+4` locates `CA06`,
and u32 `+8` locates `GS02`. The CA section ends at the GS offset.

`PC` is from what we think and infer the car itself.

## `CA06`
| Offset | Type | Meaning |
| --- | --- | --- |
| `0x10` | u16 | Node count |
| `0x12` | u16 | Material count |
| `0x1C` | u32 | CA-relative material table offset |
| `0x20` | array | Node table, stride `0x100` |

Node fields: u32 `+0` is a CA-relative mesh pointer (zero means absent)

`+0xC` contains a geometry statistic used as validity flag by the executable

`+0x40/+0x50/+0x60` are basis vectors; `+0x70` is translation

`+0x80/+0x90` are bounding-box extrema. Ignore fourth components of basis rows
when constructing an affine matrix. Parts are grouped into 16-node LOD blocks:
`node = part + 16*LOD`. Runtime selects the first group initially.

### Materials
Materials have stride `0x40`. 

Signed u16 `+2` selects a GS texture descriptor.

Byte `+4` selects a runtime primitive-alpha group: stationary wheels use group
zero and group one is their high-speed counterpart. 

U32 `+0x28` is the GS TEST
register, including alpha-test comparison, reference and failure action.


`+0x10/+0x18` receive TEX0/TEX2 during initialization. 

The u64 at `+0x38`
controls specialized drawing. Mode nibble `(flags >> 8) & 15` and shared
shared index `(flags >> 24) & 15` selects shared surfaces and effects, including
tyre tread and sidewall images. 

Mode F with flag bit 16 is disabled by the normal renderer and omitted from
visible imported geometry. Additional black wheel darkening quads with
framebuffer-only alpha-failure behavior are omitted when a coloured rim surface
already exists behind them.

### Geometry
Each packet has a `0x20`-byte header followed by VIF payload. 

Header u16 `+0`
is payload size in 16-byte quadwords and byte `+2` is type (2 in supplied meshes) and
byte `+3` counts material alternatives (1–4) and u16 material indices begin `+4`.

An all-FF 16-byte record terminates each part's stream. Next ordinary packet
starts at `current + 0x20 + qwc*16`.

VIF command words are consumed structurally, including their payload lengths.

Stuff we noticed -

| Command | VU address | Payload |
| --- | --- | --- |
| `0x6C` | `0x3FF` | Per-block control vector |
| `0x68` | `0` | Float32 XYZ positions |
| `0x69` | `0x40` | Signed int16 inward reflection vectors; negate for outward Blender normals |
| `0x64` | `0x80` | Float32 UV pairs |
| `0x6D` | TOPS-relative | Unsigned int16 position/normal/UV/control indices |

UNPACK count zero means 256. 

Index X low 13 bits selects position, Y selects
normal when that stream exists, and Z low 15 bits selects UV. 

X bit `0x8000`
suppresses triangle emission, allowing strip starts/restarts. 

Other high flags
are not fully decoded.

UNPACKs retain strip history, a new control vector resets source streams.

Index W is unsigned
baked RGB brightness, neutral at 128; alpha is separate. 

Factory reflection
weight interpolates its influence. 

## `GS02` Textures
Header u16 `+4` counts descriptors, u16 `+6` adjusts texture base, u16 `+0xC`
adds a second base for flagged descriptors. 

Descriptors start at `+0x20`,
stride `0x30`. Width/height are u16 at descriptor `+8/+0xA`, descriptor
`+0x10` is TEX0 and `+0x20` is TEX2. TEX2 supplies an effective palette override.

Texture TBP0 is relative: add header `+6`, additionally add header `+0xC` when
descriptor byte `+0xF` is nonzero. 

CBP palette addresses are separate. 

Low u16
`FFFF` at descriptor `+4` marks references that cannot be decoded from a normal
local image location, inspected materials replace these with shared runtime
effect textures. 

Descriptor `+6` is not a general descriptor reference.

---

Header u32 `+0x10` locates an inline `DMA/VIF DIRECT` upload. Its extent matches
header `+0x1C` in all files, data after that address also exists and is
not interpreted. 

Each supplied upload uses one `DMA` packet containing `GIF A+D`
register writes and IMAGE data. 

`BITBLTBUF/TRXPOS/TRXREG/TRXDIR` describe a `PSMCT32`
transfer into `GS` local memory. Uploading into simulated 4 MiB `GS` memory, then
reading through `PSMT8` (19) or `PSMT4` (20) addressing, recovers texels. 

Palettes
are `PSMCT32`, `CSM1`, index bits 3/4 swap. PS2 alpha 0–128 maps to 0–255.
For `T4` images, the game writes `TEX2` first to preload 256 colours, then `TEX0`
selects a cached 16-colour slice without reloading. 

Combine `CBP` from `TEX2` with
`CSA` from `TEX0`, using `TEX2`'s CSA loses the real rim colours and cutouts.

---

Out of the scope, but `ABCD` container has original tyre maps.

Shared indices
1–7 select bank textures 43–49 (sidewall and six tread variants); indices 8/9
select bank textures 36/35.

### Wheel Placements

| Offset | Float32 value |
| --- | --- |
| `0x10` | Wheelbase |
| `0x14`, `0x18` | Front/rear track |
| `0x3C`, `0x40`, `0x44` | Front tyre width, aspect, rim inches |
| `0x48`, `0x4C`, `0x50` | Rear equivalents |

Front centers are `(+-track/2, width*aspect + rim*0.0254/2, wheelbase)`,
rear centers use rear values and Z=0. 

Parts 1–4 are front positive-X, front
negative-X, rear positive-X, rear negative-X wheels. 

Parts 5–8 are corresponding
brakes. 

Runtime wheel transforms replace the CA fallback: static import uses
these centers with identity rotation. Right-side mirroring is already present
in the source vertices. 

The main body also uses identity at every LOD, other
rigid parts retain their source pivots.


This is static placement, not suspension or steering simulation.

### References - 
- [GS tables](https://github.com/PCSX2/pcsx2/blob/master/pcsx2/GS/GSTables.cpp).
- [PS2SDK](https://ps2dev.github.io/ps2sdk/vif__codes_8h.html).

Please note that a lot more is written and available in the `car_files` folder, but is not manually verified. AI was used in research and hence, is not in the main folder. See in [here](car_files)
- [Auxiliary Data](car_files/auxiliary_data.md)
- [Executable Data](car_files/executable_findings.md)
- [Glass Paint](car_files/glass_paint.md)
- [Wheel Data](car_files/wheel_data.md)