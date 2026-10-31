> [!WARNING]
> **Unofficial fork: RetroScaler BlueRetro adapters support**
>
> This fork adds support for the RetroScaler BlueRetro adapters: the 4 ports versions (N64, Dreamcast & GameCube with 4 BT LEDs) and the single port versions.
> It is not affiliated with, endorsed by or supported by the original BlueRetro author or by RetroScaler.
>
> RetroScaler ships these adapters with modified BlueRetro firmware but without its source code.
> The changes in this fork were reconstructed by reverse engineering their firmware binaries (4 ports images and the [single port v25.04 release](https://github.com/RetroScaler/Single-Blueretro-FW/releases/tag/v25.04)).
> Built with ESP-IDF v5.4 on top of upstream commit `ddfc31b`, these changes reproduce the RetroScaler app images byte-for-byte (except build time, version string & hashes).
> In this fork they are applied on top of the final upstream release.
>
> Which file to use (`BlueRetro_hw1_<config>_retroscaler_full.bin`):
>
> | Adapter | `<config>` |
> |---|---|
> | N64 (4 ports & single port) | `n64` |
> | Dreamcast (4 ports & single port) | `dreamcast` |
> | GameCube (4 ports & single port) | `gamecube` |
> | 3DO | `3do` |
> | Atari / SMS | `parallel_2p` |
> | SNK (Neo Geo) | `parallel_1p` |
> | MD / Genesis | `genesis` |
> | NES | `nes` |
> | SNES | `snes` |
> | PS1 / PS2 | `playstation` |
> | Saturn | `saturn` |
>
> Note: in RetroScaler's single port v25.04 release, the "GC" file is actually their Atari/SMS firmware and the "SNES" file is their NES firmware (byte-identical builds). This fork provides real GameCube & SNES builds.
>
> Changes compared to upstream BlueRetro:
> * 4 BT status LEDs on GPIO 14, 32, 33 & 25: a LED turns on once the matching BT device is identified and off when it disconnects (`CONFIG_BLUERETRO_RETROSCALER_4LED`).
> * Default accessories mode set to rumble on all ports (`CONFIG_BLUERETRO_DEFAULT_ACC_RUMBLE`).
>   * N64: each port starts with a Rumble Pak; BlueRetro's Rumble/Controller Pak toggle button (see the [wiki](https://github.com/darthcloud/BlueRetro/wiki)) switches it to a Controller Pak until the next reboot. Each port gets its own Controller Pak (4 x 32 KB).
>   * Dreamcast: port 1 gets Jump Pack + VMU (`CONFIG_BLUERETRO_DEFAULT_ACC_P1_MEM`), ports 2-4 Jump Pack only, since all ports would share the same VMU data.
>   * GameCube & PS1/PS2: rumble on all ports. Other systems have no accessories, so this setting has no effect there.
> * These accessory defaults are applied once to the existing config at boot (`CONFIG_BLUERETRO_DEFAULT_ACC_APPLY_REV`), so no reset from the web config is needed. Other settings & mappings are kept, and accessories can still be changed afterwards in the web config.
> * Build configs: `configs/hw1/<config>_retroscaler` (see table above).
>
> **Tested successfully** on the RetroScaler 4 ports N64, Dreamcast & GameCube adapters and on the single port adapters.
> The single port changes reproduce RetroScaler's Atari/SMS, SNK, MD/Genesis, NES, PS1/PS2 & Saturn images byte-for-byte (3DO only differs by its unused default accessories setting).
>
> **Download:** prebuilt firmware is available on the [Releases](https://github.com/opssemnik/Blueretro-for-retroscaler-4port/releases) page.
> * `*_full.bin` is a full flash image, the same layout as the RetroScaler images: flash it at **0x0** (e.g. NodeMCU flasher default settings).
> * `*_app.bin` is the app only: flash it at **0x10000**. Flashing it at 0x0 overwrites the bootloader and the adapter won't boot.
> * Saved settings, BT pairings & memory card data are kept when flashing either image (they are stored in a partition the images don't cover).
>
> **Build**, e.g. for N64: `cp configs/hw1/n64_retroscaler sdkconfig && idf.py reconfigure build` (ESP-IDF v5.4).
> This produces both `build/BlueRetro.bin` (app, 0x10000) and `build/BlueRetro_full.bin` (full image, 0x0).
>
> Both options are off by default, so the stock configs build the same firmware as upstream.
> Use at your own risk. Please do not report issues with this fork to the upstream project.

**This repository has been archived and is no longer under active development or maintenance.**

```
After 6 years working on BlueRetro, the time has come for me to move on
and focus on my family and my real job.

The code remains available as-is for reference, learning, and community use,
but no new features, bug fixes, or pull requests will be accepted.

Thank you to everyone who contributed, tested, reported issues,
or supported the project over the years.
Your involvement made BlueRetro what it is today.

If you are looking to build upon this work, please feel free to
fork the repository in accordance with the license.

Thank you,
Jacques Gagnon
```

# BlueRetro

<p align="center"><img src="/static/PNGs/BRE_Logo_Color_Outline.png" width="600"/></p>
<br>
<p align="justify">BlueRetro is a multiplayer Bluetooth controllers adapter for various retro game consoles & computers. Lost or broken controllers? Reproduction too expensive? Need those rare and obscure accessories? Just use the Bluetooth devices you already got! The project is open source hardware & software under the CERN-OHL-P-2.0 & Apache-2.0 licenses respectively. It's built for the popular ESP32 chip. Wii, Switch, PS3, PS4, PS5, Xbox One, Xbox Series X|S & generic HID Bluetooth (BR/EDR & LE) devices are supported. Parallel 1P (Computers, NeoGeo, Supergun, JAMMA, Handheld, etc), Parallel 2P (Atari 2600/7800, Master System, Computers, etc), NES, PCE / TG16, Mega Drive / Genesis, SNES, CD-i, 3DO, Jaguar, Saturn, PSX, PC-FX, JVS (Arcade), Virtual Boy, N64, Dreamcast, PS2, GameCube & Wii extension are supported with simultaneous 4+ players using a single adapter.</p>

## READ THIS FIRST
* [Project documentation](https://github.com/darthcloud/BlueRetro/wiki)

## Need help?
* [Open a GitHub discussion](https://github.com/darthcloud/BlueRetro/discussions)

## Makers sponsoring BlueRetro
Buying BlueRetro adapters from these makers helps support the continued development of the BlueRetro firmware!\
Thanks to all sponsors!

* [Laser Bear Industries](https://www.laserbear.net)
* [Humble Bazooka](https://www.humblebazooka.com)
* [RetroOnyx](https://www.retroonyx.com/)
* [RetroTime](https://8bitmods.com/retrotime)

## Community Contribution
* BlueRetro PS1/2 Receiver by [mi213](https://twitter.com/mi213ger): 3D printed case & PCB for building DIY PS1/2 dongle.\
  https://github.com/Micha213/BlueRetro-PS1-2-Receiver
* N64 BlueRetro Mount by [reventlow64](https://twitter.com/reventlow): 3d printed mount for ESP32-DevkitC for N64.\
  https://www.prusaprinters.org/prints/90275-nintendo-64-blueretro-bluetooth-receiver-mount
* BlueRetro Adapter Case by [Sigismond0](https://twitter.com/Sigismond0): 3d printed case for ESP32-DevkitC.\
  https://www.prusaprinters.org/prints/116729-blueretro-bluetooth-controller-adapter-case
* BlueRetro AIO by [pmgducati](https://github.com/pmgducati): BlueRetro Through-hole base and cable PCBs.\
  https://github.com/pmgducati/Blue-Retro-AIO-Units
* BlueRetro HW2 internal guides by [Nostalgic Indulgences](https://twitter.com/nosIndulgences): Internal install guides\
  https://github.com/nostalgic-indulgences/BlueRetro_Internal_Installation
* BlueRetro latency test by [GamingNJncos](https://twitter.com/GamingNJncos): Documentation on how to run BlueRetro latency test\
  https://github.com/GamingNJncos/BLE-3D-Saturn-Public/tree/main/BlueRetro_Latency_Testing
* BR4N64 by [TharathielCB](https://github.com/TharathielCB): Internal BlueRetro Flex-PCB for Nintendo 64\
  https://github.com/TharathielCB/BR4N64
* BlueMemCard by [ChrispyNugget](https://github.com/ChrispyNugget): Replacement PCB for PSX memory card that incorporates PicoMemcard and BlueRetro support\
  https://github.com/ChrispyNugget/BlueMemCard
* BlueRetro HW2 QSB for GameCube by [Arthrimus](https://github.com/Arthrimus): Internal Blueretro PCB with CurrentTrigger for GameCube\
  https://github.com/Arthrimus/BlueRetro-HW2-GameCube

<br><p align="center"><img src="https://cdn.hackaday.io/images/4560691598833898038.png" height="200"/></p>
