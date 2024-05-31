# MSX8
MSX8 CP/M PROGRAM TO LAUNCH MSX GAME ROMS

<b>IMPORTANT NOTICE</b> you will need to disconnect the front panel interrupt line of the 5 pin connector on the CPU board and jumper the VDP interrupt to INT3 on the graphics card. Follow this link: [CPU and GFX MODS](https://github.com/lesbird/MSX8/blob/main/README.md#cpu-and-graphics-modifications) - <b>The standard Z80 CPU board will not run without front panel interrupts so you will need one of my [RomWBW CPU](https://github.com/sebhc/sebhc/wiki/H8-Z80-ROMWBW-V1.0) boards to use MSX8.</b>

[MSX8.ZIP - 30KB](https://github.com/lesbird/MSX8/blob/main/MSX8.zip) the launcher including ASM source code and MSX-US.ROM for the Heathkit H8 computer. Copy these to a CP/M drive on your Heatkit computer.

[MSXROMS.ZIP - 6MB](https://drive.google.com/file/d/1CPUKjfRxF2Sq3ZCcoAHj1XeqptBqdim3/view?usp=sharing) 481 ROMs all batch renamed to CP/M friendly 8.3 format. Roms over 32K have been removed.

[ROM CROSS REFERENCE](https://github.com/lesbird/MSX8/blob/main/romlist.md) - list of game ROMs with long file names

MSX8 is a CP/M program that loads a customized MSX BIOS rom file and a MSX ROM game and then launches it. This program was designed to work with the Heathkit H8 computer with an HA8-3 Color Graphics Card (TMS9918 and AY3-8910) or with my own H8-8-3 Color Graphics card with a F18A on a Tang Nano 9K (a clone of the HA8-3).

This repository contains the source code for the CP/M program buildable with the standard CP/M ASM program as follows:

```
>ASM MSX8.AAZ
.
.
>LOAD MSX8
```

Also included in this repo is a customized FULL MSX BIOS rom that addresses the Heathkit Color Graphics card ports rather than the MSX VDP/PSG ports. The customized MSX BIOS can be built with Z80ASM included as part of the z88dk tools. Just run the make file with the paramters as follows:

```
>make TARGETS=us
```

When MSX8 is launched it jumps to high memory (0xC000) and then will look for and load the custom MSX BIOS called "msx-us.rom". This custom BIOS is loaded to a temporary address 0x0100 up to 0x3100 (12K). MSX8 will then load the GAME ROM that is passed as a parameter on the CP/M command line as follows:

```
A0>MSX8 GALAGA.ROM
```

The GAME ROM file must exist in the same folder as MSX8.COM and MSX-US.ROM. The GAME ROM is loaded at address 0x4000 up to 0xC000 (maximum ROM size is 32K). When the GAME ROM is loaded the MSX BIOS is then copied down to address 0x0000 (since we don't need CP/M anymore) and MSX8 will wait until you press L to launch the game. The game start address is retrieved from the beginning of the GAME ROM contents at address 0x4002.

```
        LHLD    4002H
        PCHL
```

At this point all control is passed to the GAME ROM with the MSX BIOS in low memory starting at 0x0000.

I have tested this with some of the popular arcade conversions for the MSX computer such as <b>PACMAN, GALAGA, GALAXIAN, DIGDUG, RALLYX, BOSCONIAN</b> and they all work perfectly. Some ROMs do not work. One in particular is FROGGER. I disassembled the FROGGER ROM and discovered it is doing direct writes to the VDP/PSG instead of going through the BIOS. I suspect many non-working games are doing this as well.

### GRAPHICS BOARD SETUP

Enable INT3 VDP interrupts on the graphics card so it will generate the 60Hz refresh rate that the games need in order to run properly.

![GFX](https://github.com/lesbird/MSX8/blob/main/MODGFX.jpg)
