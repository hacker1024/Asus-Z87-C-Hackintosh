# Asus Z87-C Hackintosh

An OpenCore configuration for my custom build with an Asus Z87-C motherboard.

## Specs

| Category                    | Component                                                    |
| --------------------------- | ------------------------------------------------------------ |
| Motherboard                 | Asus Z87-C                                                   |
| CPU                         | Intel® Core™ i7-4770                                         |
| iGPU                        | Intel® HD Graphics 4600                                      |
| dGPU #1                     | NVIDIA GeForce GT 730 (GK208B)                               |
| dGPU #2 (disabled in macOS) | NVIDIA GeForce GTX 660 Ti (GK104)                            |
| Displays                    | iGPU: Dell U2312HM (DVI); dGPU #1: Dell S2421HS (HDMI), Dell U2410 (DVI), Dell 2005FPW (VGA) |
| RAM                         | 2x 8GB DDR3 1600MHz Kingston KHX1600C10D3L/8GX<br />2x 2GB DDR3 1333MHz Kingston KP223C-ELD |
| Ethernet                    | Realtek RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller |
| Audio                       | Realtek ALC892                                               |
| SATA                        | 6x SATA 3                                                    |
| USB (EHCI)                  | 8 Series/C220 Series Chipset Family USB EHCI                 |
| USB (xHCI)                  | 8 Series/C220 Series Chipset Family USB xHCI                 |
| Wi-Fi                       | Qualcomm Atheros AR9227 Wireless Network Adapter (PCI)       |
| Bluetooth                   | Cambridge Silicon Radio Bluetooth HCI (0001:0a12), version 19.58 |

## Usage

### Instalation

1. Copy the contents of this repo to your EFI partition. Make sure to recursively clone to include submodules!
2. The `config.plist` is missing `PlatformInfo` details. Follow the [Dortania guide](https://dortania.github.io/OpenCore-Install-Guide/config.plist/ivy-bridge.html#platforminfo) to generate serials, using the `iMac15,1` model.
3. Update any kexts that you want to update.
4. Check the included SSDTs to make sure they match your ACPI information (it can change between motherboard variants and UEFI versions). Use the [Dortania guide](https://dortania.github.io/Getting-Started-With-ACPI/).
5. If you plan to use this on a USB drive, temporarily change [ScanPolicy](https://dortania.github.io/OpenCore-Install-Guide/config.plist/haswell.html#misc) to `0`.
6. Add `\EFI\OC\OpenCore.efi` (or, alternatively, `\EFI\refind\refind_x64.efi`, if you want to [dual boot](#dual-booting)) as a UEFI boot option with a tool like `bcfg` in an EFI shell or `efibootmgr` on Linux.
7. Set the UEFI settings described below.
8. Power management is configured for maximum performance. This may be changed by swapping out the `CPUFriendDataProvider` kext.
9. For better color quality, consult the [EDID documentation](EDIDs).

### Required UEFI settings

- Enable Internal Graphics: Yes (required for things like hardware video decoding)
- Pre-allocated IGPU memory (DVMT-Prealloc): 64MB
- CSM: Disabled
- EHCI handoff: Enabled if macOS won't boot with it off

## Notes

### Dual booting

To dual boot with other operating systems, I suggest one of these two methods:

1. Use the UEFI boot selection screen by pressing F8 when you want to boot into a different OS.
2. Use an initial bootloader like rEFInd to choose an OS on startup.

Personally, I go with option 2, as I need to use Windows a lot, and pressing F8 every time gets a bit tedious.

A copy of rEFInd is included in this reporitory, with the following configuration:
- My fork of [refind-theme-regular](https://github.com/hacker1024/refind-theme-regular) is used as the theme
- OS scanning is disabled
- Mouse input is enabled
- OpenCore and Windows menuentries are configured
- The last booted entry (either OpenCore or Windows) is automatically booted after 20s. This is nice for updates.
- NVRAM is used for rEFInd storage

Feel free to configure rEFInd to your liking.  
If you use rEFInd, make sure to initialize this repository's submodules (or clone it recursively).

### Hardware quirks

- The Atheros AR9227 PCI Wi-Fi card can cause extreme instability in multi-core systems!
  I think this may be related to the inclusion or exclusion of a PCI-to-PCI bridge.
  
  If your system freezes to the point where even the reset button doesn't work, this card is the likely culprit.
  Remove the kext, or disconnect the card entirely as this issue also effects Windows (though Linux seems to be fine).

  This card also has a unique PCI device ID; the included High Sierra Atheros plugin is modified to use it.
