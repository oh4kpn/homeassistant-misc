# Home Assistant Configuration for CO2-Based Fan Control

This repository contains a Home Assistant configuration that adjusts the fan speed based on CO2 levels in a room.
Warning: ChatGPT was used here

## Structure

- **automations/fan_control.yaml**: Contains the automation for adjusting the fan speed steplessly based on CO2 levels.
- **sensors/makuuhuone_fan_percentage.yaml**: Optional Template sensor configuration for displaying the fan speed percentage.
- **esp32_codes/makkari.esp32.conf**: Optional Template for adding configuration for esphome with esp32 hardware

## How It Works

- The automation triggers whenever the CO2 sensor (`sensor.olohuone2022_makuuhuone2022_carbon_dioxide`) changes.
- The fan (`fan.esp32_fan_makkari_makuuhuone_fan_pwm`) speed is adjusted smoothly between 0% (at 500 ppm) and 100% (at 900 ppm).
- Optional: A template sensor displays the current fan speed percentage on the dashboard.

## Installation

1. Copy the files to your Home Assistant configuration directory.
2. Include these files in your main `configuration.yaml`:
   ```yaml
   automation: !include_dir_merge_list automations/
   sensor: !include_dir_merge_list sensors/
   ```
3. Restart Home Assistant and verify the configuration.

## Notes

- Ensure your filenames and paths are correct and don't contain unsupported characters like `-`.
- Adjust entity IDs as needed for your setup.
