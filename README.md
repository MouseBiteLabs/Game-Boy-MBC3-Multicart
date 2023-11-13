# Game Boy MBC3 Multicart - UNDER CONSTRUCTION

This is my design of a flashable MBC3-based multicart for the Game Boy. You can make a 2-in-1 or 4-in-1 cartridge, that changes games based on a button press or power cycling the Game Boy. With this board you can also make a single game, but with the addition of a pressable reset button on the cartridge.

The features are as follows:

- Able to make a cartridge with either 2x 16 Mbit games or 4x 1Mbit games
- Optionally adds a pressable button to the cartridge (no shell cutting required)
  - The button can be configured to cycle games on the cart, reset the console, or both
- Configurable to have separate save data for each game, or to share the same save data across multiple games
- Fully compatible with the <a href="https://www.gbxcart.com/">GBxCart RW</a> so you can transfer games and save files to and from the board

Some examples of fun cartridges you can make with this board are:
- A single Pokemon game that has a reset button
- Both Pokemon Gold and Silver with separate saves
- Both Pokemon Gold and Silver with *the same* save (save files are compatible between versions!)
- Both Pokemon Gold and Silver with *the same* save that can switch between versions *during gameplay without resetting* (hotswap!)
- A single copy of Mary-Kate and Ashley - Pocket Planner with two separate save files

![image](https://github.com/MouseBiteLabs/Game-Boy-MBC3-Multicart/assets/97127539/f0deac10-d3ee-4818-bb71-8c292750321e)

All gerbers and source files can be found in this repo, as this project is fully open source. Technical documentation of the board can be found in the Technical folder.

## Important Things Before You Start

1) To use this board, you need to have an original Game Boy game that uses an MBC3 mapper chip. <a href="https://catskull.net/gb-rom-database/">You can find a list of games and their mappers here</a>. Use the search function.
2) You will need to remove the MBC3 from your donor cartridge for use on this board. This will require a hot air rework station or a hot plate. There's a list below of other parts you can re-use from the donor cartridge.
3) When soldering parts on, it's a good idea to put kapton tape or otherwise cover the bottom cartridge edge. You do not want to get solder on the cartridge contacts.
4) I am not responsible for any damage you do to your self or your property. I do not guarantee design compatibility. You may encounter issues with certain games! Attempt this project at your own risk.
5) If you are using this board to make games other than for personal use, you must have permission from the originator to use and distribute any ROM images or other related material. You are responsible for making sure you adhere to any license requirements.

DO NOT use my circuit boards for profiting from stolen work - this especially includes homebrew content, ROM hacks, and using fan-made labels without permission from the originator. **Support original creators!**

## Board Characteristics and Order Information

The zipped folder contains all the gerber files for this board. The following options **must** be chosen when ordering boards for yourself.

- Thickness: 0.8mm
- Surface Finish: ENIG
- Gold Fingers: Yes, 30° chamfer

**Add this note to orders:** "The milling.gbr layer indicates where an unplated slot should be added to the board. Please add an unplated slot as indicated."

**Currently not selling on Etsy, but will in the future. Stay tuned.**

<a href="https://www.etsy.com/shop/MouseBiteLabs"><img src="https://github-production-user-asset-6210df.s3.amazonaws.com/97127539/239718536-5c9aefe3-0628-4434-b8d8-55ff80ac3bbc.png" alt="PCB from Etsy" /></a> 

You can use the zipped folder at any board fabricator you like. You may also buy the board from PCBWay using this link (disclosure: I receive 10% of the sale value to go twoards future PCB orders of my own):

[link]

<a href="">The board is also listed on OSH Park as well.</a> **Be sure to get them in 0.8mm thickness if you order from here.**

## Required Equipment

The EEPROMs on the board needs to be programmed somehow. I recommend using the GBxCart, as mentioned. These boards are fully compatible with it, and it makes reflashing games extremely easy using <a href="https://github.com/lesserkuma/FlashGBX">lesserkuma's FlashGBX software</a>.

Alternatively, you can buy an EEPROM programmer with a TSOP adapter. The downside to this method is that you have to desolder the chip every time you want to program it. The <a href="https://flashcatusb.com/">FlashcatUSB</a> is one popular option in retro spaces.

## Battery Safety

When assembling a board, I recommend populating all the parts *except* the battery, and getting it to run initially without it. This is to make it easier to fix any solder connections that might need fixing, without having to worry about getting the battery hot. And if you need to rework anything near the battery after you've put it on the board, be safe and remove it before putting a hot soldering iron next to it.

And this should go without saying, but if you're assembling these boards with a hot plate or hot air, *do not* solder the battery on this way. You should use an iron, and keep heat off of the battery as much as possible. 

(Also check polarity!)

## Board Configurations

Ooh boy.

## Test Points and Final Checkout

On the back of the board are five test points. Here's where they are connected:

- TP1: SRAM supply voltage
- TP2: Battery voltage (after R1)
- TP3: Battery voltage (positive terminal of battery)
- TP4: Ground
- TP5: VCC input voltage

After you assemble your game, you should measure the current out of the battery. But first, you should program it with the GBxCart, or if you programmed the EEPROM separately, put it into a Game Boy and cycle power once. Then, flip the PCB upside down on a non-conductive surface (not your leg), and set your multimeter in DC millivolts (or volts). Put the positive probe on TP3 and the negative probe on TP2. If you used a 10kΩ for R1, as indicated in the BOM, you should read a voltage in the single or tens of millivolts for non-RTC games, or up to 60 mV for games using a real-time clock. If you have something much higher, especially voltages above 60mV, then you likely have an issue or short circuit on the board somewhere.

### Current Draw Measurements

The revision of MBC3 chip you are using will influence how much current draw you get, and thus how long your battery life will last. For the test set up, I am replacing the battery with a regulated DC power supply set for 3 VDC for consistency, on a cart board with an MM1134 chip for U4, and brand new AS6C62256 SRAM.

| Rev   | P/N      | Current draw (no RTC) | Current draw (with RTC) |
| ----- | -------- | --------------------- | ----------------------- |
| MBC3  | LR385364 |          ?             |            ?             |
| MBC3  | BU3631K  |          ?             |            ?             |
| MBC3A | LR38536B |          ?             |            ?             |
| MBC3A | BU3632K  |          ?             |            ?             |
| MBC3A | P-2      |          ?             |            ?             |
| MBC3B | BU3634K  |          ?             |             ?            |
| MBC3B | P-2      |          ?            |              ?           |

## Bill of Materials (BOM)



### Usable Donor Cartridge Parts

You can use a few parts from the donor cart on the new board to save some money. Note that you will generally get better reliability with new parts as opposed to old ones. For example: I have seen failed RAM chips from donors in the past.

1) **C2, C3, R2, X1: RTC Components** - You can move over these parts if you're using the real-time clock function
2) **U2: MBC3** - This one is required
3) **U3: SRAM** - You can use this part *only if* the sum of the RAM space for the games you're using is the same or less amount of RAM that the donor cartridge does

You could probably transfer over most of the 0.1uF capacitors but they're pretty cheap anyway, so I generally just recommend buying new resistors and capacitors.

## Things to Remember

- The 29F032 and 29F033 have been known to occasionally be defective upon arrival. They're either used, or new old stock, and usually only available from AliExpress.
- The footprint for the battery can fit a CR2032, CR2025, or CR2016 with solder tabs. The only difference is the mAh capacity (larger number = longer life). If you get Panasonic tabbed batteries, you may have to trim the battery tabs to make them fit on the footprint.
  - For untabbed coin cells, you can find battery retainer adapters online, <a href="https://retrogamerepairshop.com/products/hdr-game-boy-game-battery-retainer?variant=40511013290156">like this one.</a>
- Kb is kilo**bits** and Mb is mega**bits**. Sometimes you will find game ROM and RAM sizes defined in terms of KB or kilo**bytes** and MB or mega**bytes**. You can convert Kb and Mb to KB and MB by dividing Kb or Mb by 8. For example, 256 Kb = 32 KB.

## Revision History

### v1.1
- 
### v1.0
- Prototype revision

## Resources and Acknowledgements

- <a href="http://www.devrs.com/gb/files/hardware.html">Jeff Frohwein's GameBoy Tech Page</a>
- <a href="https://gbhwdb.gekkio.fi/">Game Boy Hardware Database</a>
- <a href="https://catskull.net/gb-rom-database/">Nintendo Gameboy Game List</a>
- <a href="https://www.gbxcart.com/">insideGadgets discord server for GBxCart RW compatibility requirements</a>
- <a href="https://github.com/lesserkuma/FlashGBX">Lesserkuma's FlashGBX software</a>
- <a href="https://www.ti.com/lit/ds/symlink/tps3613-01.pdf?HQS=dis-mous-null-mousermode-dsf-pf-null-wwe&ts=1698238885366&ref_url=https%253A%252F%252Feu.mouser.com%252F">TPS3613 Datasheet</a>
- Board outline modified from <a href="https://tinkerer.us/projects/homebrew-gameboy-cartridge.html">Dillon Nichols's Homebrew Gameboy Cartridge project</a>
- Some components from <a href="https://github.com/adafruit/Adafruit-Eagle-Library">Adafruit's Eagle parts library</a>
- Some components from <a href="https://www.snapeda.com/">SnapMagic</a>
- Thank you to <a href="https://github.com/Gekkio">gekkio</a> for their deep Game Boy knowledge resources, and for collaboration in demystifying some of the design choices on Game Boy cartridges
- Thanks to the awesome members of the <a href="https://moddedgameboy.club/">Modded Gameboy Club</a> for their feedback and support during the entire project development

## License

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/80x15.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>. You are able to copy and redistribute the material in any medium or format, as well as remix, transform, or build upon the material for any purpose (even commercial) - but you **must** give appropriate credit, provide a link to the license, and indicate if any changes were made.

©MouseBiteLabs 2023
