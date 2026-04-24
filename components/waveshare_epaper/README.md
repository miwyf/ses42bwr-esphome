# 4.2in E-Paper External Component

This folder contains a local ESPHome `external_components` override for `waveshare_epaper`.

It adds extra 4.2-inch models:

- `ses42bwr`
- `blozi42`

Current scope:

- Black/white only
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

Also adapted for:

- Brand: BLOZI
- Model: Endor
- Screen/FPC marking: `P420016-MF1-A`
- Model name in YAML: `blozi42`

Not included:

- Partial refresh
- BWR/red plane support
- Upstream compatibility guarantee across future ESPHome releases

## Files

- `display.py`: registers the `ses42bwr` and `blozi42` models
- `waveshare_epaper.h`: declares the `Ses42BWR` and `Blozi42` display classes
- `waveshare_epaper.cpp`: validated 4.2-inch init/LUT sequence, full refresh logic, horizontal mirror fix

## How To Use

In your YAML, point `external_components` to your local `components` folder and select `model: ses42bwr` or `model: blozi42`.

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

## Notes

- The current implementation mirrors the X axis in software to match the tested 4.2-inch tag wiring/orientation.
- `full_update_every` is accepted by the YAML because ESPHome expects it, but these models currently behave as full refresh only.
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
