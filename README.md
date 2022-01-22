https://github.com/pfgithub/w4test

# wasm4image
image converter and compressor for wasm4 zig

`wasm4image in.png out.w4i`

supports:

- png, jpg, … anything stb_image supports

options:

- `--compress`: compresses the output image
- `--detect-palette`: automatically detects the color palette and puts it at the start of the output file

examples:

- https://wasm4.org/play/image-carousel
  - ![image](https://user-images.githubusercontent.com/6010774/150019992-def6a6d8-e422-4417-886b-d2c47fd06721.png)
  - to make this, I:
    - found images on unsplash
    - resized them down to 160x160
    - used https://www.imgonline.com.ua/eng/limit-color-number.php to convert them to 4 color, 100 quality jpegs
      - I'm sure there is some command line tool or something you can use to do this better
    - converted to w4i using --compress --detect-palette
  - source code is available in this repository

## usage (from zig)

### uncompressed image:

without embedded palette:

```zig
const img = @embedFile("image.zig");
w4.blit(…, 2bpp);
```

with embedded palette

```zig
const img = w4i.loadImage(@embedFile("image.zig"));
// img.data: []const u8
// img.palette: [4]u24
w4.PALETTE.* = img.palette
```

## usage (from c)

embed the file somehow

call

```c
w4i_decompress(
    image_data,
    // size:
    50, 50,
    // write to buffer:
    tex,
    // write at offset in buffer:
    0, 0,
);
```
