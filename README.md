# Patch TGM PSX on macOS

Building the tools to apply this patch correctly requires xcode to be installed.

### Make a directory to work in

```
mkdir -p Projects/tgm-psx-patch && cd Projects/tgm-psx-patch
```

### Extract patch

Extract files into directory named `patch`.

```
unzip ~/Downloads/tgm_psx_v1.2a.zip -d patch
```

### Extract ROM
(ate-05m.3h and ate-06m.4h)

```
unzip ~/Downloads/tgmj.zip -d patch
./patch/extract_tgm.sh
```

### Apply patches to MAIN.BIN

Using an IPS patcher utility, patch `MAIN.BIN` with `tgm_psx_v1.2a.ips`

#### (Optional) Apply CD-DA music patch

Using an IPS patcher utility, patch the already patched `MAIN.BIN` with `cdda_music.ips`

##### Download vgm rip

Download from here: https://vgmrips.net/files/Arcade/Capcom/Tetris_-_The_Grand_Master_%28ZN-2%29.zip

Extract files into patch directory:

```
unzip ~/Downloads/Tetris_-_The_Grand_Master_\(ZN-2\).zip -d patch
```

##### Setup - Install VGMPlay

Follow the instructions at https://github.com/vgmrips/vgmplay/tree/0.40.9#compile-vgmplay-under-macos to compile VGMPlay.

The script assumes `vgm2wav` is in a specific directory, so we will install it there.

```
mkdir -p patch/vgmplay-0.40.9/VGMPlay
cp ../vgmplay/VGMPlay/vgm2wav patch/vgmplay-0.40.9/VGMPlay/
```

##### Convert the music

Run the convert script. This might take a minute.
```
cd patch
./convert_tgm_music.sh
```

### Convert patched MAIN.BIN to MAIN.EXE

First compile bin2exe_tgm.c

```
make bin2exe_tgm
```

Now convert MAIN.BIN
```
./bin2exe_tgm ../TGM_ROM_DATA/MAIN.BIN ../TGM_ROM_DATA/MAIN.EXE
```

### Master disc
Download mkpsxiso from https://github.com/simonlc/mkpsxiso/releases/download/v1.24/mkpsxiso

```
mv ~/Downloads/mkpsxiso patch/
cd patch
chmod +x mkpsxiso
brew install tinyxml2
```

Obtain LICENSEA.DAT found in this archive https://docs.google.com/uc?export=download&confirm=G9cM&id=0B_GAaDjR83rLZGVaZ2pvV2tjSVE

It is found in `cdgen/LCNSFILE/LICENSEA.DAT`
```
cp ~/Downloads/psyq/cdgen/LCNSFILE/LICENSEA.DAT .
```

```
mv ../TGM_ROM_DATA .
```

Copy SYSTEM.CFN into rom data directory:
```
cp SYSTEM.CNF TGM_ROM_DATA/
```

**NOTE:** If mkpsxiso is blocked, open "Security & Privacy" in system preferences and Allow it to run.

```
./mkpsxiso TGM_PSX_CDDA.xml
```

If the CD-DA patch was not used, run this:

```
./mkpsxiso TGM_PSX.xml
```

### Play

You now have TGM_PSX_CDDA.cue and TGM_PSX_CDDA.bin rom files. These can be burned to a disk or played with an emulator.
