# Auxiliary car resources

> AI Generated and Researched, beware.

Evidence is from the local US executable and all 211 supplied car packages. Addresses are executable virtual addresses. Original assets and original IDA database were not changed. `tools/enthusia_importer/extras.py` exposes bounded parsers; `tests/test_extras.py` exercises source topology, clip holes, malformed references, truncated records, nonfinite data and hierarchy cycles.

## Section two is shadow geometry, not collision

The resource object created by `sub_2420D8` uses vtable `0x46E000`. Its type-information function `sub_358230` references the literal `16RyuCarShadowDt_t` at `0x489A48`. Labeling this low-resolution body shell as collision would be incorrect. It is suitable for a hidden auxiliary mesh, not a visible opaque body replacement.

The loader at `0x242128` through `0x242218` establishes this layout:

| Offset | Field |
|---|---|
| `0x00` | u32 version/magic 18101 |
| `0x04` | u32 vertex count; runtime rejects counts >= 96 |
| `0x08` | u32 edge-record count |
| `0x0C` | u32 triangle count |
| `0x10` | u32 four-index strip count |
| `0x14` | u32 variable-strip count |
| `0x20` | vertex count * vec4 float positions |
| after vertices | two vec4 bounding minimum/maximum |
| after bounds | edge count * four u32 records |
| after edges | triangle count * three u32 indices |
| after triangles | four-index strip count * four u32 indices |
| after quads | variable-strip count * `(u32 count,u32 section-relative offset)` |

The first two edge-record fields reference vertices. Other edge fields are preserved without invented meanings. Debug drawing in `sub_241808` uses the triangle table at resource `+0x44`, four-index strip table at `+0x48`, and variable strips at `+0x4C`. Four-index records are triangle strips, so their two triangles are `(a,b,c)` and `(c,b,d)`; treating the record as a cyclic four-sided polygon changes topology. Bit 31 in index records can be used as a runtime face flag and is removed when resolving vertex references.

All 211 resources parse: 9,776 vertices and 14,743 triangles after strip conversion. Six cars have variable strips. Example `00000003.car`: 42 vertices, 91 edge records, 42 triangles, eight four-index strips, no variable strips; resource size `0xB08`.

## Section three contains glare contours

`sub_246408` creates the object using vtable `0x46E168`. Its type function `sub_3586C8` references `16RyuGlareGeomDt_t` at `0x489B18`. The renderer `sub_246178` iterates these groups and calls `sub_244F10`, which uses camera position/direction, car transforms, contour intersections and material color to generate a view-dependent highlight. These are not collision hulls or ordinary textured meshes.

Loader `sub_2464D8` and record initializer `sub_2463C8` establish:

```text
u32 magic/version = 90101
u32 group_count
group_count * (u32 kind, u32 point_count)
align16
for each group:
    vec4 anchor
    point_count * vec4 positions
    point_count * vec4 direction/control vectors
```

The shader can close a contour when its kind is nonzero (`0x245338`); every supplied group's kind is zero. Preserve the control vectors without normalizing them: source magnitudes vary. All 211 resources parse: 1,720 groups and 10,738 contour points. Importing hidden polylines, anchors and vector metadata preserves this data without fabricating the dynamic glare effect.

## Section one contains rigid animation

This optional section is an FC87 package. It exists in 22 of 211 cars. Entry zero contains a skeleton; subsequent entries are animation clips. Empty entries are real slots and must retain their numbering: cars `00000292.car` and `00000334.car` have empty slots before later clips.

`sub_1924E8` loads section one through `sub_1941D0`. Generic package loader `sub_2429C0` provides its entries to `sub_2474A8`; that function loads the skeleton with `sub_246EE8` and clips with `sub_2472D0` / `sub_23CD28`.

Skeleton layout established by `sub_246EE8`:

```text
u32 magic/version = 0x00100002
u32 bone_count
u32 extra_vector_count
u32 extra_scalar_count
extra_vector_count * vec4 defaults
extra_scalar_count * u32 defaults
bone_count * s32 parent indices (-1 = root)
bone_count * s32 output IDs
align16
bone_count * matrix4x4 float bind matrices
optional per-bone extra defaults
```

All supplied skeletons have zero extra-vector/scalar counts, one to four bones and root parents only. The parser explicitly rejects unsupported extra-default layouts and compressed keys rather than quietly inventing values. Duplicate output IDs occur in some source skeletons and are retained; the runtime lookup in `sub_194448` chooses the first bone with a matching output ID.

Each nonempty clip starts:

```text
u32 magic/version = 0x01100002
float duration
u32 bone_count
u32 extra_vector_count = 0
u32 extra_scalar_count = 0
for each bone:
    s32 channel_counts[4]
    for each nonempty channel:
        float key_times[count]
        align16
        vec4 key_values[count]
```

The four channels are translation, quaternion rotation in XYZW order, scale, and Euler rotation. `sub_23D6B0` evaluates channel one and passes it to the quaternion-to-matrix formula `sub_25D0E0`; channel two multiplies matrix basis vectors, and channel zero supplies the translation at matrix `+0x30`. Channel three uses the Euler rotation helper `sub_25CAB8`. Supplied clips use translation and quaternion channels only. The parser retains every authored time/value instead of synthesizing samples.

`sub_194568` applies six resulting output matrices to model part slots nine through fourteen, so an output ID maps to `part = ID + 9`. This is rigid part animation; no per-vertex skin weights were found in these resources. Runtime behavior includes retractable components and active aero: `sub_1927E0` contains a speed-dependent 95 km/h trigger for one model. Avoid naming every clip a headlight animation.

Matrix convention matters: `sub_25D0E0` stores cross terms with signs opposite the matrix produced by a standard Blender `(w,x,y,z)` quaternion when using the importer's existing raw-matrix transpose. Conjugate the source quaternion, then apply the existing game-to-Blender axis change. Check an animated component against its source bind position before changing visible placement.

Corpus totals: 22 skeletons, 33 bones, 40 nonempty clips and 212 vector/quaternion keys. `read_animations` returns `None` for an empty section; clip holes remain `None`, and each nonempty clip retains its original nested-entry index.

## CA06 placement markers

CA header `+0x18` stores a u16 section-relative table offset; byte `+0x1A` stores its record count. Each record is 16 bytes: three float coordinates and one packed u32 kind. `sub_262AF0`, instructions `0x262BF0` / `0x262BF8`, rebase the pointer into renderer `+0x20` and copy the count to renderer byte two. The gap before geometry is not a table of 48-byte transforms: sample `00000003.car` has five 16-byte markers at CA `0x5320` through `0x5360`, followed by padding.

`sub_1EAF68` reads marker byte `+13` (kind's second byte) and collects categories zero, one and three for the light-glow renderer `sub_1EBC40`. That renderer selects distinct source constants initialized by `sub_1EC138`:

| Kind category (`kind >> 8`) | Original glow RGBA | Interpretation |
|---|---|---|
| 0 | 138,128,108,128 | Warm front-light glow |
| 1 | 96,8,0,128 | Red rear-light glow |
| 3 | 108,108,128,128 | Cool front-light glow |

`sub_2008B0` selects category one with first kind byte one or two and creates the two rear-light trail resources. Category two is separately collected by `sub_1E9268` (maximum four markers); `sub_1E9BC8`, `0x1EA7D4` onward, uses those positions in an engine-state-dependent particle-emitter path. Their low rear-pipe positions support exhaust/backfire anchors. The helper retains raw kind names/values rather than assigning unverified side labels or simulating light/particle behavior.

## Blender helper validation

`blender_extras.import_extras(context, collection, car, filepath, axis)` creates hidden shadow, glare, placement-marker and rigid-animation helpers in a source-named Data collection. The source JSON text retains parsed vec4 data, raw resource bytes, original clip slots and physics parameters. It does not create a collision mesh.

Animation tracks start muted, with frames `1 + seconds * scene_fps / fps_base`. Unmuting a source clip plays original translation, conjugated quaternion and scale keys with linear interpolation. Euler keys remain in source JSON because none of the supplied clips exercises that channel. Source part output IDs bind the first matching bone to parts nine through fourteen across imported LODs; initial model world matrices are preserved.

Installed Blender 5.2.3 LTS verified all 22 animation-bearing cars: source first/end key values, static placement preservation, clip holes, muted defaults and cleanup after a simulated final JSON-write failure. A nonanimated NISMO sample also passes. New tracks are evaluated once before muting so Blender establishes their animation dependencies; creating them muted from the outset left fresh-import playback unevaluated. Other Blender versions were not exercised.

## Remaining limits

These identified resources do not establish a collision mesh. The physics parameter block and the game's live physics remain separate. Exact glare rendering, shadow projection, runtime animation triggers, clip interpolation and missing per-bone extra-default formats are not reproduced by the auxiliary parsers. Native mesh LODs, UVs and normal vectors are documented separately; none of the material paths inspected establishes tangent-space normal-map strength.
