# 4.2in / 7.4in E-Paper External Component

This folder contains a local ESPHome `external_components` override for `waveshare_epaper`.

It adds extra models:

- `ses42`
- `ses42bwr`
- `blozi42`
- `blozi42bwr`
- `ses74bwr`

Current scope:

- `ses42` & `blozi42`: Black/white (two-color)
- `ses42bwr` & `blozi42bwr`: Black/white/red (three-color)
- `ses74bwr`: Black/white/red (three-color), 7.4", 800 × 480
- Full refresh only
- ESP8266 verified to compile
- Based on the ESPHome `waveshare_epaper` architecture

## Verified Panel

Tested with:

- Brand: SES-imagotag / VUSION
- Size: 4.2"
- Type: BWR panel
- Product code: `R42A01101`
- FPC/screen cable marking: `A1360071-00`

Model names in YAML:

- `ses42`: legacy mono rendering path for the SES 4.2 panel
- `ses42bwr`: Z21-based black/white/red rendering path

Also adapted for:

- Brand: BLOZI
- Model: Endor
- Screen/FPC marking: `P420016-MF1-A`
- Model names in YAML: `blozi42` (mono) or `blozi42bwr` (BWR rendering path)

Also added:

- Brand: SES-imagotag / VUSION
- Size: 7.4"
- Type: BWR panel
- Controller: Pervasive E2741CS0Bx
- Resolution: 800 × 480
- Model name in YAML: `ses74bwr`

Not included:

- Partial refresh
- Upstream compatibility guarantee across future ESPHome releases

## Files

- `display.py`: registers the `ses42`, `ses42bwr`, `blozi42`, `blozi42bwr` and `ses74bwr` models
- `waveshare_epaper.h`: declares the `Ses42`, `Ses42BWR`, `Blozi42`, `Blozi42BWR` and `Ses74BWR` display classes
- `waveshare_epaper.cpp`: validated init/LUT sequences, full refresh logic, horizontal mirror fix

## How To Use

In your YAML, point `external_components` to your local `components` folder and select the appropriate model.

Example:

```yaml
external_components:
  - source:
      type: local
      path: components
    components: [waveshare_epaper]

spi:
  clk_pin: GPIO14
  mosi_pin: GPIO13

display:
  - platform: waveshare_epaper
    id: eink_display
    cs_pin: GPIO15
    dc_pin: GPIO0
    busy_pin: GPIO4
    reset_pin: GPIO2
    model: blozi42
    update_interval: 30s
    lambda: |-
      it.fill(COLOR_OFF);
      it.print(10, 10, id(my_font), COLOR_ON, "BLOZI Endor 4.2");
```

Example for `ses74bwr` (SES 7.4" BWR panel, 800 × 480):

```yaml
external_components:
  - source:
      type: local
      path: components
    components: [waveshare_epaper]

spi:
  clk_pin: GPIO14
  mosi_pin: GPIO13

display:
  - platform: waveshare_epaper
    id: eink_display
    cs_pin: GPIO15
    dc_pin: GPIO0
    busy_pin: GPIO4
    reset_pin: GPIO2
    model: ses74bwr
    update_interval: 30s
    lambda: |-
      it.fill(COLOR_OFF);
      it.print(10, 10, id(my_font), COLOR_ON, "SES 7.4 BWR");
```

## Notes

- The current implementation mirrors the X axis in software to match the tested panel wiring/orientation.
- `full_update_every` is accepted by the YAML because ESPHome expects it, but these models currently behave as full refresh only.
- `ses74bwr` uses the Pervasive E2741CS0Bx controller with an 800 × 480 resolution and a dedicated index/data SPI protocol.
- If a future ESPHome version changes the internal `waveshare_epaper` API, this local override may need to be rebased.

## Build

Validate, compile, and upload using your own YAML file:

```powershell
esphome config your-device.yaml
esphome compile your-device.yaml
esphome upload your-device.yaml --device YOUR_PORT
```

## Repository

This README documents the component itself. A publishable repository can place this folder under:

```text
components/waveshare_epaper/
```
