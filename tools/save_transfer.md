# Save Transfer
Save transfers from version to version are possible in Enthusia.

This means transferring saves from 
- `US <-> JP`
- `US <-> EU`
- `EU <-> JP`

This is possible using some simple `hex` editing

Let us take example of `US -> JP` in this case, thought it will be same for basically all of them

## Requirements
- Save from original region (`US`)
- `mymc` (Tool for converting and importing and extracting save data)
  - Download from http://www.csclub.uwaterloo.ca:11068/mymc/
- A hex editor (preferably `HxD`, but https://hexed.it/ also works) 

## Steps
- Get the original save region (`US` in this case) and if not yet converted, convert it into a `.ps2` format
- Open `mymc-gui.exe`
- It should open up and ask to `Open Memory Card Image`. Point this to the original memory card (`US`)
- You should see a save saying (in this case) `BASLUS-20967SYBO0` and such in a line. This is the Enthusia save
- Select the save and press the `Export Button` or `File > Export` and save the `EMS save file (.psu)` somewhere
- Close `mymc`
- Now, you need to open your preferred Hex Editor. I will show both the methods below
  - `HexEd.it Website`
    - Open the https://hexed.it/ site
    - Click on `Open File` and open the `.psu` file
  - `HxD`
    - Open the app and drag the `.psu` file in it
- Now, depending on region, this step varies a lot 
  - If you are editing `US` file, search for all occurances of `BASLUS-20967` in the Hex file
  - If you are editing `JP` file, search for all occurances of `BISLPM-65948` in the Hex file
  - If you are editing `EU` file, search for all occurance of `BESLES-53125` in the Hex file
- Now, to convert it to the region you want to convert change all occurances of `Bxxxxx-xxxxx` thing text from the previous step (should be 3 occurances), to the one below depending on which region you want to convert it into
  - To convert to `US`, replace by `BASLUS-20967`
  - To convert to `JP`, replace by `BISLPM-65948`
  - To convert to `EU`, replace by `BESLES-53125`
- After doing this, save the file.
  - So for example, you want to convert `US` to `JP`
    - You find all occurances of `BASLUS-20967` (there should be 3)
    - You replace all occurances of `BASLUS-20967` with `BISLPM-65948`
 - So for example, you want to convert `EU` to `US`
    - You find all occurances of `BESLES-53125` (there should be 3)
    - You replace all occurances of `BESLES-53125` with `BASLUS-20967`
- After saving the file, open `mymc-gui.exe` and open the `.ps2` memory card file that you opened before
  - So that means it should have the line (in case of `US` region) `BASLUS-20967SYBO0`
- Now, press the `Import Button` or `File > Import`
- Import the `.psu` file that you saved from the Hex Editor
- If you were editing from `US to JP`, it should make a new line with `BISLPM-65948SYBO0` alongside the original `BASLUS-20967SYBO0`
- Now close `mymc` and insert the `.ps2` memory card in PCSX2
- Open the region you converted your rave into and it should work just fine