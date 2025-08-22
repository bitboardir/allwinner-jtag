# Allwinner JTAG Adapter

This project aims to create a general-purpose JTAG adapter for boards using **Allwinner SoCs**. These SoCs often share JTAG signals with SD card signals, so the adapter is designed to resemble a **micro SD card** at the end.

## Design Decisions

- **Form Factor:**  
  The PCB is designed in the form of a micro SD card, allowing it to slide directly into a micro SD card slot. This makes the adapter extremely convenient and easy to use.

- **USB to JTAG Adapter:**  
  The **FT232H** is used as the USB-to-JTAG adapter. This choice leverages the large ecosystem of FTDI-based JTAG adapters and provides a low-cost solution. OpenOCD supports FT232H adapters out of the box with minimal configuration.

- **Voltage Level Shifters:**  
  All four JTAG lines include voltage level shifters. This serves two purposes:  
  1. **Voltage isolation:** Each side is protected from potential voltage spikes or damaged components.  
  2. **Prevention of parasitic powering via SD pins:** This allows the JTAG adapter to remain plugged in at all times without interfering with the board’s normal operation.

- **No EEPROM for FTDI:**  
  To simplify the design, no EEPROM is included for storing custom USB configurations (VID, PID, description string, serial number). For hobbyist use, the default FTDI configuration is sufficient for JTAG operations.

## OpenOCD Configuration

A reasonably new version of OpenOCD is recommended (currently **0.12.0**). For 64-bit Allwinner SoCs that boot in **AArch32 mode**, the current git version is preferred, as it includes fixes for this mode.  

The adapter can be used with the following OpenOCD settings:

```text
adapter speed 1000
adapter driver ftdi
ftdi vid_pid 0x0403 0x6014
ftdi layout_init 0x0008 0x000b
transport select jtag
```


![Board schematic](/pic/schematic.png)
