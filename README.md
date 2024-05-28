# MSX8
MSX8 CP/M PROGRAM TO LAUNCH MSX GAME ROMS

[MSXROMS.ZIP - 10MB](https://drive.google.com/file/d/1jMZHKzHdKzh-uVd8EpNnj2vCAhP1Kqdi/view?usp=sharing) all batch renamed to CP/M friendly 8.3 format. Roms over 32K have been removed.

MSX8 is a CP/M program that loads a customized MSX BIOS rom file and a MSX ROM game and then launches it. This program was designed to work with the Heathkit H8 computer with an HA8-3 Color Graphics Card (TMS9918 and AY3-8910) or with my own H8-8-3 Color Graphics card with a F18A on a Tang Nano 9K.

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

The GAME ROM file must exist in the same folder as MSX8.COM and MSX-US.ROM. The GAME ROM is loaded at address 0x4000 up to 0xC000 (maximum ROM size is 32K). When the GAME ROM is loaded the MSX BIOS is then copied down to address 0x0000 (since we don't need CP/M anymore) and MSX8 will wait until you
press L to launch the game. The game start address is retrieved from the beginning of the GAME ROM contents at address 0x4002.

```
        LHLD    4002H
        PCHL
```

At this point all control is passed to the GAME ROM with the MSX BIOS in low memory starting at 0x0000.

I have tested this with some of the popular arcade conversions for the MSX computer such as <b>PACMAN, GALAGA, GALAXIAN, DIGDUG, RALLYX, BOSCONIAN</b> and more and they all work perfectly. Some ROMs will not run for whatever reason, which could potentially be that they are addressing the hardware directly instead of through the MSX BIOS.
