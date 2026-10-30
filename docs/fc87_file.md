# FC87

This game tends to use a lot of FC87 container files. See for example anything in `\pack\carpk` under `.car` files.

# How It Works
Let us take an example of `00000003.car` from `\pack\carpk`
```hex
87 FC 00 10 00 00 00 00 06 00 00 00 40 00 00 00 
60 8B 0A 00 A0 8B 0A 00 10 02 00 00 B0 8D 0A 00 
08 0B 00 00 C0 98 0A 00 50 07 00 00 10 A0 0A 00 
E8 01 00 00 00 A2 0A 00 0F 01 00 00 00 00 00 00
```
All is in little endian.

The magic number is `87 FC 00 00` that shows that this is a `FC87` container.

At `u32 + 8`, we find the `Section / Chunk Count`, `06 00 00 00`. As visible here, there are 6 chunks in this particular example.

Starting from `0xC`, the entries for the sections start. As you can see from example, the first `u32` is the `Relative File Offset` and the next `u32` is the `Relative File Size`.

For example, first entry `0` starts at `40 00 00 00` with a relative size of `60 8B 0A 00`.

Then the next entry `1` starts at `A0 8B 0A 00` with a relative size of `10 02 00 00`.

This goes on until all entry tuples are filled. If space is left, it pads the header with `00 00 00 00`.