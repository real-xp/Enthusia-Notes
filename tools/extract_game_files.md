# Extract Game Files
Extracting the game files from the `.iso` of the game is not a hard thing to do, thanks to Nenkai's [Very Useful Tool (GitHub Link)](https://github.com/Nenkai/EnthusiaVolumeFS)

This tool can be used to extract game files of 
```
SLPM_68519 (Subaru Demo)
SLPM_65948 (Retail Japan)
SLUS_20967 (Retail US)
SLES_53125 (Retail Europe)

(source is the readme.md from the github repo)
```

Please note that this is not a complete document.

# Steps
- Go to https://github.com/Nenkai/EnthusiaVolumeFS > Releases > `EnthusiaVolumeFS.exe.zip` and download it
- Extract the zip file in a folder
- Open `cmd` in your folder where you extracted the files and type
  - `EnthusiaVolumeFS.exe <path-to-d000.bin> <output-directory> <game-type>`
    - in place of `<path-to-d000.bin>`, replace your game's extracted `.iso` path like `D:\Enthusia\ENTHUSIA\D000.BIN`
    - in place of `<output-directory>`, put where you want to extract files, like `D:\EnthusiaExtractedFile\`
    - in place of `<game-type>`, place the version you want to replace, so for US, put `SLUS_20967`
  - Final command should look like `EnthusiaVolumeFS.exe D:\Enthusia\ENTHUSIA\D000.BIN D:\EnthusiaExtractedFile\ SLUS_20967`
    - Note that if you have a directory path with spaces, put the directory in double quotes like `"D:\Enthusia Extracted File\"`
    - Note that if you want to output the log to a `.txt` file, after the command type ` > <path-to-string-file>` where `<path-to-string-file>` is something like `D:/Enthusia_log_file.txt`
      - So command looks like `EnthusiaVolumeFS.exe D:\Enthusia\ENTHUSIA\D000.BIN D:\EnthusiaExtractedFile\ SLUS_20967 > D:/Enthusia_log_file.txt`
- It should take around a few seconds to extract, and after that you will see all files extracted

# Open TIM2 Format
Some files in `/pack/` folder are containing `TIM2` format images, which is a PS2 Image format. Read more at [The VG Resource Wiki](https://wiki.vg-resource.com/TIM2)

You can extract the `TIM2` format using the steps from [here](https://wiki.vg-resource.com/PlayStation_2#Texture_Extractor.2FReinserter_.28TextER.29)

To view the extracted `TIM2` files, you can use `Noesis`. Again, tutorial found [here](https://wiki.vg-resource.com/Noesis)