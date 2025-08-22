Allwinner JTAG adapter
This project aims to create common JTAG adapter for boards which use Allwinner SoCs. These SoCs commonly mix JTAG signals with SD card signals, so this adapter board looks like micro SD card at the end.

Design decisions
PCB in form of micro SD card is pretty obvious choice. End of adapter will simply slide into micro SD card slot, which makes it easy to use.

FT232H as USB to JTAG adapter is also obvious choice as there is already pretty big ecosystem of FTDI based JTAG adapters and it's low cost solution. OpenOCD supports such adapters out of the box with minimal configuration needed.

Last but not least, all four JTAG lines use voltage level shifter. This serves two purposes. First is to isolate voltages to some degree. Each side is protected from other in case of any voltage spikes or burnt components. Second reason is to prevent parasitic powering chip via SD pins. This allows to have JTAG adapter plugged in at all times, without worries of interferring with board operation.

In order to simplify things, there is no EEPROM for FTDI. This prevents storing custom configuration like USB VID&PID, description string and custom serial. However, since this is meant just for hobby and defaults are fine for JTAG use, this is reasonable compromise.

OpenOCD configuration
Reasonably new version of OpenOCD is recommended (currently 0.12.0). For 64-bit Allwinner SoCs which boot to AArch32 mode first current git version is even better since there are fixes for this mode of operation. Next release will have this fixes included. This adapter can be used with following settings:

adapter speed 1000
adapter driver ftdi
ftdi vid_pid 0x0403 0x6014
ftdi layout_init 0x0008 0x000b
transport select jtag

NOTE: Speeds higher than 1 MHz were not tested.
