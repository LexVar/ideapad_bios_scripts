
# Ideapad 5 BIOS Control Scripts

This are just some simple scripts to control some BIOS settings of the Lenovo Ideapad 5 Pro 14ACN6 (It also works for other models like the ideapad 5 14ARE05).

The settings are changed using the acpi_call kernel module and the calls specified in the [archwiki page](https://wiki.archlinux.org/title/Lenovo_IdeaPad_5_14are05#Tips_and_tricks).

## Available Settings

- Power mode (controls the system fan):
    - Intelligent cooling
    - Battery saving
    - Extreme performance
- Rapid charging
- Conserve battery (stops charging when connected to power at 60%)

I also include a script `conserve_battery` to turn on the battery conservation mode at 80% battery instead of 60% so when I unplug the laptop from power it has more charge. The script was made to be run in a cron job every X minutes.

## Installation

If you have the acpi_call kernel module installed you can just clone the repository and run the scripts.

    git clone <url>

To install the `acpi_call` kernel module clone its [repository](https://github.com/nix-community/acpi_call) and run:

    # Compile Module
    make
    sudo make install
    # Load the kernel module
    sudo modprobe acpi_call

Note: you have to run these commands for every new kernel you install.

## Usage/Examples

    sudo ./bios-power-mode <mode>

The `mode` value can be either:
- `INTELLIGENT_COOLING`
- `POWER_SAVING`
- `EXTREME_PERFORMANCE`
- `RAPID_CHARGING`

Not specifying a value will show the mode currently enabled.

### Automate changing power modes

You can include udev rules in `/etc/udev/rules.d/10-power.rules` to change the BIOS power mode when the laptop is plugged/unplugged from power.

```
SUBSYSTEM=="power_supply", ENV{POWER_SUPPLY_STATUS}=="Discharging", RUN+="/usr/local/bin/bios-power-mode BATTERY_SAVING"
SUBSYSTEM=="power_supply", ENV{POWER_SUPPLY_STATUS}=="Unknown", RUN+="/usr/local/bin/bios-power-mode INTELLIGENT_COOLING"
```

## To do

- [ ] Toggle rapid charging (currently only turns on)
