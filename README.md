# Z87-C Hackintosh

An OpenCore configuration for my custom build with an Asus Z87-C motherboard.

## Specs

| Category                    | Component                                                    |
| --------------------------- | ------------------------------------------------------------ |
| Motherboard                 | Asus Z87-C                                                   |
| CPU                         | Intel® Core™ i7-4770                                         |
| iGPU                        | Intel® HD Graphics 4600                                      |
| dGPU #1                     | NVIDIA GeForce GT 730 (GK208B)                               |
| dGPU #2 (disabled in macOS) | NVIDIA GeForce GTX 660 Ti (GK104)                            |
| RAM                         | 2x 8GB DDR3 1600MHz Kingston KHX1600C10D3L/8GX<br />2x 2GB DDR3 1333MHz Kingston KP223C-ELD |
| Ethernet                    | Realtek RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller |
| Audio                       | Realtek ALC892                                               |
| SATA                        | 6x SATA 3                                                    |
| USB (EHCI)                  | 8 Series/C220 Series Chipset Family USB EHCI                 |
| USB (xHCI)                  | 8 Series/C220 Series Chipset Family USB xHCI                 |
| Wi-Fi                       | Qualcomm Atheros AR9227 Wireless Network Adapter (PCI)       |
| Bluetooth                   | Cambridge Silicon Radio Bluetooth HCI (0001:0a12), version 19.58 |

## Usage

1. Copy the contents of this repo to your EFI partition.
2. The `config.plist` is missing `PlatformInfo` details. Follow the [Dortania guide](https://dortania.github.io/OpenCore-Install-Guide/config.plist/ivy-bridge.html#platforminfo) to generate serials, using the `iMac15,1` model.
3. Update any kexts that you want to update.
4. Check the included SSDTs to make sure they match your ACPI information (it can change between motherboard variants and UEFI versions). Use the [Dortania guide](https://dortania.github.io/Getting-Started-With-ACPI/).
5. If you plan to use this on a USB drive, temporarily change [ScanPolicy](https://dortania.github.io/OpenCore-Install-Guide/config.plist/haswell.html#misc) to `0`.
6. Add `\EFI\OC\OpenCore.efi` as a UEFI boot option with a tool like `bcfg` in an EFI shell or `efibootmgr` on Linux.
7. Set the UEFI settings described below.
8. Power management is configured for maximum performance. This may be changed by swapping out the `CPUFriendDataProvider` kext.

## Required UEFI settings

- Enable Internal Graphics: Yes (required for things like hardware video decoding)
- Pre-allocated IGPU memory (DVMT-Prealloc): 64MB
- CSM: Disabled
- EHCI handoff: Enabled if macOS won't boot with it off
