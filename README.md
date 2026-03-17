# Open PS2 Loader

Copyright © 2013-2022, Ifcaro & jimmikaelkael.
Licenced under AFL v3.0 - Review the LICENSE file for further details.

This is a modified version of Open PS2 Loader [v1.0.0](https://github.com/ps2homebrew/Open-PS2-Loader/tree/v1.0.0).</br>
Created for the PSX DVR (DESR) consoles that have issues booting games from internal HDD using [OPL-Launcher](https://github.com/ps2homebrew/OPL-Launcher).

This updated and modified version includes:
- OPL-Launcher support
- HDL & NBD server support
- Controller Settings Menu
- Gamepad Macros (Modification of Gamepads - Requires PADEMU variant)
- OSD Language Configuration Menu
- Apps Menu
- Expanded SMB Ports
- Allows usage of other partitions than +OPL
- Fixes & Updates from the latest Open PS2 Loader release

Thanks to ackmax, AKuHAK, bignaux, israpps, KrahJohlito, uyjulian and all other contributors of Open PS2 Loader !

---

### About This Fork

Forked from SvenGDK's as it was included with PFS Batchkit (https://github.com/GDX-X/PFS-BatchKit-Manager) and was more stable with my PS2 build than latest stable or dev builds by PS2Homebrew (https://github.com/ps2homebrew/Open-PS2-Loader) on the date of 3/17/2026

Tested and deployed on:
PS2 SCPH 5001(dex)
Running FreeMCBoot/FMCB 1.9
OEM PS2 HDD Network adapter w/ Bitfunx FAT v2.0 Adapter Mod
2TB Crucial BX500 SATA 2.5 inch with 3D printed adapter, traditional PFS PS2 partionioning for stability.

Intention was to create a direct OPL boot PS2/PS1 library that could be parent locked down so it all "just works".
UX was prime concern. Features needed to include adding app_case PNG support for PS1 POPSTARTER APPS/ELF section (stable 1.1 has it but this is a pre 1.1 fork), automatic default VMC use, so no manual setup of memory cards per game, disabling of certain menu items when parental lock is on, adding art control fixes such as hiding or displaying discs or covers and background art on info page, coloring of some other text items, multiplayer info in cfgs to display on info page (replacing Size: key), renaming of pages to declare a PS1/PS2 section of games, automatic powering down after set time limit, minor bug fixes, and more.

---

<details>
  <summary> <b> Fork Features </b> </summary>
<p>

This fork adds the following features on top of SvenGDK's build:

#### appsMain Theme Support
- Themes can define separate main and info page layouts for the APPS menu using `appsMain` and `appsInfo` element prefixes in the theme config
- APPS cover art uses a dedicated `apps_case` overlay for POPSTARTER jewel case style artwork

#### Theme Customization (Display Settings > Theme Customization)
- Show/hide: Game ID, Page Title, Hints Bar, Menu Icons, Info BG Art
- Per-mode show/hide for Cover Art and Disc Icon (BDM, HDD, ETH, APP)
- Page Title Color and Hint Text Color pickers
- Game list dynamically recalculates height when elements are hidden

#### Custom Page Names (Settings > Extra Settings)
- Rename any mode's page title (e.g. "HDD Games" → "PS2", "Apps" → "PS1")
- Stored in `conf_opl.cfg`, leave empty to use the default

#### Remember Last Played — APPS/POPSTARTER Fix
- APPS mode now saves `last_played` using the app title for unique matching (POPSTARTER games share ELF names)
- All modes save `last_played_mode` so OPL boots to the correct page on startup
- Default Device is greyed out when Remember Last Played is active

#### wLaunchELF Integration (Settings > Extra Settings)
- Configurable path to wLaunchELF, appears in the main menu when set
- Validates the file exists before launching, deinits OPL cleanly

#### Default VMC (Settings > Extra Settings)
- Global default VMC Slot 1 and Slot 2, auto-applied on game launch
- Only applies when a per-game VMC is not already configured

#### Parental Lock Improvements
- Sensitive menu items are hidden entirely when locked (not just password-gated)
- Locked menu shows only: Parental Lock, About, Exit, Power Off
- Full menu appears immediately after successful unlock
- Fixed: Start NBD Server was always visible regardless of HDD mode

#### Info Page Improvements
- Fixed two missing APPS badges: `#Format` (ELF) and `#Media` (defaults to CD)
- Players data replaces the Size line when `Players=` key exists in the game cfg
- Tiered parsing: `Players=players/N` → `PlayersText=N` fallback → keep original Size
- Works across all modes: HDD, BDM, ETH, and APPS

#### Development Notes
Portions of this fork were developed with AI assistance (Claude, Anthropic). All code was reviewed, tested, and validated on PS2 hardware.

</p>
</details>

<details>
  <summary> <b> Releases </b> </summary>
<p>

When you download and extract the latest Open PS2 Loader from this repo, you will receive 5 variants:

| Variant | File Name | Description |
| --------- | ----------- | ----------- |
| `Release` | OPNPS2LD.ELF | Regular OPNPS2LD release with GSM. |
| `ALL` | OPNPS2LD-ALL.ELF | With GSM and all the features below. |
| `IGS` | OPNPS2LD-IGS.ELF | With In-Game Screenshot feature. |
| `PADEMU` | OPNPS2LD-PADEMU.ELF | With Pad emulation for DS3 & DS4. |
| `RTL` | OPNPS2LD-RTL.ELF | With Right-To-Left language support. |

</p>
</details>

<details>
  <summary> <b> USB Support </b> </summary>
<p>

- USB drives must be formatted in FAT32 (MBR)
- You can format large drives in FAT32 with http://ridgecrop.co.uk/index.htm?guiformat.htm
- USBUtil is the recommended tool to install a game on USB drives, it can split games over 4GB. </br> You can get it here: https://www.psx-place.com/threads/usbutil-by-iseko.19048/

</p>
</details>

<details>
  <summary> <b> How to use the NBD server </b> </summary>
<p>

- To connect to the NBD server you will first need an NBD client and driver on your PC.
- The Ceph MSI installer bundles a signed version of the WNBD driver. </br> It can be downloaded from here: https://cloudbase.it/ceph-for-windows/
- Install the client and reboot.
- Open CMD as administrator and run: </br> ```wnbd-client.exe map hdd1 SERVER_IP``` <- Shown in OPL when NBD server started.
- You can now install games with hdl_dump like: </br> ```hdl_dump inject_dvd hdd1: "GAME_TITLE" "ISO_FILE" "BLUS_123.45" *u4```
  - This process can take up to 1-2 hours depending on game size.
- If you want to disconnect from the NBD server, open a Command Prompt as administrator and run: </br> ```wnbd-client.exe unmap hdd1```

</p>
</details>

<details>
  <summary> <b> How to compile this version </b> </summary>
<p>

#### Using Docker (recommended)

```sh
# Pull the PS2 dev Docker image
docker pull ps2dev/ps2dev:v1.2.0

# Launch the container from the repo root
docker run -it -v "${PWD}:/src" ps2dev/ps2dev:v1.2.0 sh

# Inside the container:
apk add build-base git zip

# Patch missing USB definitions
cat >> $PS2SDK/common/include/usbhdfsd-common.h << 'EOF'
#define USBMASS_IOCTL_CHECK_CHAIN 0x0004
#define USBMASS_IOCTL_GET_FRAGLIST 0x0005
#define USBMASS_IOCTL_GET_DEVICE_NUMBER 0x0006
EOF

# Build
cd /src
make clean
make
```

#### Manual Setup

- Install ps2dev requirements
- Use [ps2dev v1.2.0](https://github.com/ps2dev/ps2dev/releases/tag/v1.2.0)
- Add following code to $PS2SDK/common/include/usbhdfsd-common.h

```
/** Check if fragments exist */
#define USBMASS_IOCTL_CHECK_CHAIN    0x0004
/** Return fragment table **/
#define USBMASS_IOCTL_GET_FRAGLIST   0x0005
/** Get the device number for the block device backing the mass partition */
#define USBMASS_IOCTL_GET_DEVICE_NUMBER     0x0006
```

#### Compile all variants
```make all-variants```
#### Compile with Right-To-Left (RTL) language support
```make RTL=1```
#### Compile with In Game Screenshot (IGS)
```make IGS=1```
#### Compile with Pad Emulator (PADEMU)
```make PADEMU=1```
#### Compile and compress elf with ps2-packer
```make NOT_PACKED=0```
#### Compile uncompressed elf
```make```

</p>
</details>
