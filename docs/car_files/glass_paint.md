# Factory paint and glass evidence

> AI Generated and Researched, beware

Target: US `SLUS_209.67`, inspected through idalib using a workspace copy of the existing IDB. Car examples are `00000003.car` and `00000160.car`. No source assets or original IDB changed.

## Factory variants live in car section five

`sub_2D15F8` returns the first u32 of the car-color resource as its variant count. `sub_2D12C8` (normal car path) and `sub_2D1148` (alternate rendering path) index variants with stride 52. `sub_1931B0` clamps the selected variant to zero through count minus one before applying it. The ordinary loader uses the resource exposed by `sub_192E08`; the matching data in each examined `.car` is package section five.

Section layout:

| Field | Offset |
| --- | --- |
| Variant count u32 | `0` |
| Variant k metadata u32 | `4 + 52*k` |
| Variant k name offset u32, relative to section | `8 + 52*k` |
| Variant k color group g, 11 bytes; g=0..3 | `12 + 52*k + 11*g` |

`sub_2D1550` returns the relocated name pointer from the second variant field, supporting the name-offset interpretation. Do not treat the first metadata u32 as RGBA.

For each group, bytes 0..3 are the first RGBA vector. Bytes 4..6 plus byte 10 are the second RGBA vector. Bytes 7..9 form the third RGB vector. `sub_2616E8`, instructions `0x26175C` through `0x261770`, converts component bytes to floats by dividing by 128 and stores three vectors at renderer `+560 + 48*g + 16*vector`. These are modulation factors, not conventional 0..255 display RGB; values above 128 are possible. The third vector's fourth argument is taken from variant metadata, but its significance was not established and should not be interpreted as ordinary opacity.

Sample `00000003.car` contains four named factory variants: Classic Red, Brilliant Black, Montego Blue Mica and Chaste White. Their first group diffuse RGB bytes are respectively `(158,9,32)`, `(12,12,14)`, `(20,30,50)` and `(239,235,229)`; alpha is 128 in all four. Preserving these source values and names gives actual available paint choices. File order index zero is a reproducible import default; it is not a claim about the player's last chosen color.

## Material group binding and layering

Source material byte `+5` is a signed color-group index. `0xFF` means no factory color group. The runtime type is initialized at material `+6` according to flags at `+0x38`: ordinary surface type zero uses vector zero, environment/reflection type one uses vector one, and fixed-color type two uses vector two. `sub_261E88`, instructions `0x2624E8` through `0x26254C`, selects renderer `+560 + 48*group + 16*runtimeType` and multiplies it into the supplied draw color. Material groups therefore tint the texture/shading passes; replacing a complete textured surface with flat factory RGB loses detail.

Factory group numbers are not semantic material labels. In the Mazda, group zero binds painted body material 18, while group one binds material 7 on the soft-top/upper rear roof. Group one's first vector is `(42,41,37,28)`, unchanged across paint variants. It initially looked like glass from a coarse bounding box, but its source alpha blending is disabled and the actual window geometry is elsewhere. **Do not make every group-one material transparent.**

Reflections can use separate material alternatives on the same geometry. A static Blender material may approximate these with a single surface using original texture, factory modulation and ordinary roughness/specular settings. It does not reproduce the game's complete ordered pass composition or lighting.

## Alpha blending has an explicit flag

`sub_262AF0`, instructions `0x262D48` through `0x262D88`, reads bit 12 of the material flags and shifts it into bit six of the eventual GIF PRIM value. `sub_261E88` places that primitive value in GIF-tag PRIM bits starting at 47. GS PRIM bit six is ABE, alpha-blend enable. Thus:

`alpha_blending_enabled = bool(material_flags & 0x1000)`

This separates body material 18 (`...8002`, blending off) from ordinary window material 0 (`...9012`, blending on) in the Mazda. Factory/vector alpha alone does not imply surface transparency when blending is off.

GS material fields and draw writes:

| Source field | Runtime GS register | Evidence |
| --- | --- | --- |
| u64 `+0x20` | ALPHA_1, `0x42` | `0x26249C` / `0x2624A8` |
| u32 `+0x28` | TEST_1, `0x47` | `0x2624B8` / `0x2624C4` |
| u64 `+0x30` | CLAMP_1, `0x08` | `0x2624D4` / `0x2624E0` |

`+0x2C` is not CLAMP. Register names/bit layouts are corroborated by [PCSX2 GSRegs.h](https://github.com/PCSX2/pcsx2/blob/master/pcsx2/GS/GSRegs.h).

ALPHA `0x44` selects standard source-alpha composition: `(sourceRGB - destinationRGB) * sourceAlpha / 128 + destinationRGB`. This is separate from alpha testing. TEST AFAIL one means framebuffer-only writes for failed alpha tests: the color is still blended/written while depth is not. Therefore applying binary alpha clipping to every alpha-tested material is incorrect.

## Original window textures contain tint and opacity

Mazda window geometry resides in node/part 14, source bounds X about ±0.704, Y 0.807..1.176, Z -0.022..1.745. Materials 0 and 28 use texture 43, flags `0x002B000000009012`, TEST `0x5160B` and no factory group. Texture 43 contains original dark teal RGB `(8,15,15)`, pane alpha 128 after the importer's PS2-alpha conversion (raw 64), and opaque border alpha 255. Smooth texture alpha should allow the pane to remain transparent while preserving the border.

NISMO `00000160.car` has glass in its body node: material 104, texture 40, flags ending `9012`, TEST `0x5160B`, no factory group. The source contains dark blue-green pane RGB `(4,9,11)` with converted alpha 204, warm areas `(12,9,8)` with alpha 212, and opaque black borders. These values supply original tint and relative opacity; no arbitrary glass color is required.

TEST `0x5160B` enables GEQUAL reference 96 but has AFAIL framebuffer-only. Keep smooth color alpha for the visible glass approximation. By contrast, rim TEST `0x5060B` uses AFAIL KEEP and supports actual cutout coverage. Ordinary opaque body materials have ABE off, so their texture alpha can carry paint/blend data without making the entire roof transparent.

## UV and sampling notes

`sub_262AF0` initializes ordinary materials' runtime CLAMP to zero (repeat U/V), mode A/B/C to five (clamp U/V), and mode D to a generated region clamp value. Reading only the source `+0x30` word without applying mode initialization can therefore be misleading. Environment/reflection passes use renderer-generated coordinates and should not be diagnosed as ordinary mesh-UV errors solely from a white reflection fallback.

Wheel transforms are another independent issue: the game replaces wheel matrices through `sub_261658` / `sub_193B48`. Raw right-wheel geometry can already be mirrored, so applying the fallback CA rotation a second time reverses rim placement. A neutral import must use the runtime neutral transform convention rather than blindly applying all CA fallback bases.

### Corpus UV audit

All 9,078 GS02 texture descriptors in the 211 supplied cars have width/height exactly equal to `1 << TEX0.TW` / `1 << TEX0.TH`. No padding-driven UV scale correction is needed. A scan of 4,480,061 decoded UV corners found every value on the source's 1/1024 coordinate grid. 281,926 corners lie outside the unit square, including negative edge coordinates and intentional repeats. Globally clamping these to zero through one would change source mapping.

The mesh stream supplies VIF `V2_32` UV pairs (`UNPACK 0x64`, address `0x80`) and separate per-corner UV indices from `V4_16` index records. Their high `0x8000` bit is removed before indexing. The normal CPU draw path sends the mesh payload directly by DMA (`sub_261E88`, `0x262604` through `0x262620`); no CPU-side per-texture scaling or half-texel correction was found. Preserve the authored float values rather than adding an assumed offset.

Vertical orientation was checked against the Mazda front license plate. Material 9 uses texture 36; front upper vertices near game Y `0.4936` use source V `0.9375`, while lower vertices near Y `0.3413` use source V zero. The stored texture image itself is vertically inverted. Importer's image-row reversal and mesh mapping `(U, 1-V)` together produce upright, readable EUNOS/ROADSTER lettering in the Blender preview. Removing either conversion changes that result. This is a concrete source-quad check, not an assumption that every decoded image should look upright in an image viewer.

1,340 corners have extreme finite values greater than 16, confined to four source cars. They are not arithmetic introduced by the importer: for example `00000020.car` at file offset `0x7370` contains literal float pair `(0,9722392)` following VIF command `0x64074080`. Several such materials are opaque nearly-uniform color stand-ins; other cases are lower LODs or disabled masks. These source anomalies were retained rather than silently remapped. Their intended authoring history was not established. No broad UV-normalization fix is justified by them.

### Mode-F mask layers

Mode F initializes runtime material type five, clears TEX0/TEX2 and selects an untextured primitive. In the draw path, `0x26224C` clears the pass's texture-register block, `0x262264` selects GS RGBAQ and `0x262268` writes its value as zero. Flag bit 16 gates optional runtime passes; it does not make every mode-F surface invisible. An earlier blanket omission was incorrect.

Mazda material 10 contains 553 body/undertray/interior occluder triangles, source W zero, flags `0x0001000000028F02`, ABE disabled, gate bit 16 disabled, and TEST `0x50000` with alpha testing disabled. The normal material loop writes ALPHA, TEST and CLAMP but no material-specific FRAME/FBMSK. Together with the untextured zero RGB, this supports an opaque black occluder under the ordinary framebuffer state. Omitting it exposes the studio floor through the cabin. Retain ungated nonwheel mode-F geometry as matte black, while optional gated passes and wheel-mask passes remain specialized. Preserve those source meshes hidden as auxiliary render-pass data; exact game framebuffer effects are not reproduced.

Glass can contain coincident inward and outward source faces. Mazda node 14 has 78 inward faces in material 0 with no normal stream, plus 78 outward faces in material 28 with a normal stream. The corresponding first triangles have identical coordinates and opposite cross products. Rendering both sides of both transparent surfaces causes duplicated tint and coplanar artifacts. Preserve their source orientation and use single-sided glass rendering; globally reversing every no-normal face would break the intentional inside pane.

## Per-corner baked brightness

The fourth component of each `V4_16` index record is a baked RGB brightness value, not an extra UV index or alpha. Its normal value is 128; values below/above 128 darken/brighten the source color. This is established by the car-specific VU program, whose first upload begins at executable address `0x372838`, loaded to VU instruction zero. The earlier program beginning `0x370558` is different and must not be used to interpret the car dispatch table at `0x372720`.

Relevant car-program instructions (addresses identify the lower word of each eight-byte VU pair):

| Executable address | Operation | Meaning |
|---|---|---|
| `0x372BE0` | `LQ.w vf17,0(vi4)` | Read original index W |
| `0x372C00` | `ITOF0.w vf17,vf17` | Convert integer brightness to float |
| `0x372BD8` | `SUBw.w vf16,vf0,vf3` | `vf16.w = 1-k`, because constant `vf0.w` is one |
| `0x372C18` | `MULx.w vf16,vf16,vf26` | Multiply by constant 128 |
| `0x372C30` | `MULAw.w ACC,vf16,vf0` | Start accumulator with `128*(1-k)` |
| `0x372C38` | `MADDw.w vf15,vf17,vf3` | Add `W*k` |
| `0x372C68` | `MAXx.w vf15,vf15,vf0` | Clamp brightness at zero |
| `0x372C80` | `MULw.w vf2,vf23,vf0` | Output alpha comes from draw alpha, independently of W |
| `0x372C88` | `MULw.xyz vf2,vf23,vf15` | Multiply RGB by brightness |
| `0x372CA8` | `MINIz.xyzw vf2,vf2,vf26` | Clamp output channels at 255 |
| `0x372CC0` | `SQ.w vf15,-1(vi4)` | Save adjusted float brightness for subsequent material passes |

CPU `0x262470` initializes control `k` to float one. For a bound factory group, CPU `0x26251C` replaces it with renderer field `588 + 48*group`, i.e. the fourth component of that group's second vector: source group byte ten divided by 128. This control is separate from RGB vector selection at `0x262548`.

For an ordinary diffuse pass, the recovered normalized brightness factor is therefore:

```text
k = factory_group[10] / 128, or 1 when no factory group is bound
brightness = max(0, 128*(1-k) + index_W*k)
RGB_factor = brightness / 128
```

The game multiplies this into RGB before GS texture modulation. W does not reduce coverage or alpha. Do not clamp the factor at one: values over 128 are present in authored body and soft-top vertices. Later environment passes reuse the adjusted brightness saved above; their generated UV/lighting is a separate approximation.

This explains white-texture interior geometry: its authored shading resides in index W. Preserve W per corner, including where one position has distinct W values in separate index records. A Blender shader should account for the fact that GS modulation operates on encoded color-channel values; multiplying a linear-light Principled input by the same numeric factor is a lighting approximation, especially for very dark corners. Opcode interpretation was checked against [PCSX2 VU disassembly tables](https://github.com/PCSX2/pcsx2/blob/master/pcsx2/DebugTools/DisVUmicro.h) and [operand definitions](https://github.com/PCSX2/pcsx2/blob/master/pcsx2/DebugTools/DisVUops.h).

## Source normal direction convention

The signed V3_16 stream points inward on ordinary exterior surfaces. It must be negated when used as a Blender outward shading normal. This is independent of the axis conversion: the existing `(x,-z,y)` basis has positive determinant and cannot account for this reversal.

Concrete source examples from `00000003.car`: the upper soft-top near `(0,1.2265,0.5193)` has source direction approximately `(0,-0.9783,0.207)`; the positive-X outer body near `X=0.785` has source direction `(-0.811,-0.567,-0.144)`. Left outer-rim geometry near local `X=+0.078` has average source X direction `-0.9647`; the right rim near `X=-0.0776` has average source X direction `+0.9726`. NISMO `00000160.car` and `00000334.car` have the same opposed outer-rim directions.

A body-node corpus check excluded mode-F masks and selected predominantly vertical roof normals in the top tenth of each body's height, plus predominantly lateral normals on the outermost tenth of its width. Results before conversion: 39,982 selected roof corners point downward versus 1,680 upward; 71,654 outer-side corners point inward versus 1,816 outward. Every one of the 211 cars has a majority of inward selected side directions. Interior surfaces and small details account for local opposite directions; an isolated inner-panel sample is not a reliable global-convention test.

The VU code preserves the source sign: executable `0x372B18` converts the source signed normal components with `ITOF15`, and `0x372B38` stores converted XYZ. Environment mapping reads those components at `0x372E98`, transforms them at `0x372EC0` through `0x372ED8`, and applies its own coordinate bias. No blanket negative-one normal conversion was found in that path. Ordinary diffuse RGB uses the separate baked W brightness described above, not a source-normal diffuse dot product. The stream therefore serves the game's inward reflection-direction convention; directly treating it as Blender outward normals produced inside-out body geometry when face winding was aligned to it.

For Blender, negate provided nonzero directions, then align triangle winding to the converted directions. Packets without a normal stream must retain their source strip winding; negating zero vectors provides no orientation evidence and does not justify flipping such faces. This correction does not require changing UV orientation or mirroring the coordinate basis.

## Ordered ordinary material passes can contain detail

The geometry header's one-to-four material indices are ordered draw passes. They are not generally mutually exclusive alternatives. `sub_261E88` iterates the count (`0x2620EC` onward), looks up each indexed material at `0x26211C`, independently applies its group-alpha condition, and appends each enabled pass. Primitive-alpha groups two through seven include conditional additive lamp states; group fifteen does not select only one ordinary material.

Two ordinary, source-alpha-blended group-fifteen passes can contain different authored bitmaps. Concrete decoded examples:

| Car | First ordinary pass | Later ordinary pass | Detail lost by selecting only the first |
|---|---|---|---|
| `00000011.car` | Material 23, texture 4: black cutout/vent marks | Material 70, texture 5 | Gray door-handle finish and keyhole |
| `00000010.car` | Material 28, texture 20: black window mask | Material 42, texture 19 | Gray window region and horizontal defroster lines |
| `00000032.car` | Material 89, texture 34: rear-window mask | Material 100, texture 33 | Colored defroster stripes, including RGB `(160,96,97)` |

The examples' later ordinary passes use group fifteen and GS ALPHA `0x44`, so they are ordinary source-alpha composition, not disabled lamp states. Ordered image composition must account for each pass's own tint and test/blend state; approximating the intervening environment-map pass does not justify discarding unrelated RGB detail.

An all-211-car scan requiring later mode-zero, non-shared, group-fifteen passes with ABE enabled and ALPHA `0x44` finds 1,794 affected batches and 519 primary-material identities. Of those primary identities, 85 occur with conflicting detail chains: a single globally modified material is insufficient. For example, `00000011.car` uses primary 23 both alone and followed by 70, while primary 22 can be followed by either 71 or 73. Material variants must be keyed by ordered batch chain. Among 1,794 later-pass occurrences, 1,784 bind the same factory group as the first material and ten differ: `00000114.car` 54→74, `00000170.car` 29→64, and `00000327.car` 38→65 change factory group zero to unbound `0xFF`.

`ordinary_detail_chain` in `materials.py` selects this conservative static subset; `blend_encoded_rgb` evaluates `(Cs-Cd)*As + Cd` on encoded normalized RGB, with PS2 alpha 128 represented by one. The ALPHA register's unused FIX byte does not affect this source-alpha equation. Neither helper infers Blender transparency from framebuffer alpha. Surface-coverage composition is a separate approximation. Each ordinary texture must receive its own factory RGB modulation; the first ordinary VU pass computes and saves adjusted W brightness for subsequent passes, so independently applying a different factory group's W coefficient to a later pass is not an exact emulation.

Other common later passes are entirely black RGB with authored alpha, such as Mazda material 56 / texture 44 after glass material 28 / texture 43. Those attenuate existing framebuffer/reflection color. Conditional lamp materials commonly use ALPHA `0x48`, which adds source-alpha-weighted RGB to the destination. ALPHA `0x58` instead involves destination alpha. These classes must not all be treated as interchangeable diffuse image layers. Some references require runtime textures absent from the car package.

## Limits

Factory values, group binding, alpha enable, tint pixels, register offsets and variants are established. Exact ordered reflections, environment coordinates, paint highlights, gamma/lighting, and destination-alpha operations remain approximations in a standard Blender shader. Static factory modulation and source glass transparency should be described accordingly.
