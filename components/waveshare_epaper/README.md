# SES42BWR External Component

This folder contains a local ESPHome `external_components` override for `waveshare_epaper`.

It adds one extra model:

- `ses42bwr`

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

Not included:

- Partial refresh
- BWR/red plane support
- Upstream compatibility guarantee across future ESPHome releases

## Files

- `display.py`: registers the `ses42bwr` model
- `waveshare_epaper.h`: declares the `Ses42BWR` display class
- `waveshare_epaper.cpp`: SES42BWR init sequence, LUTs, full refresh logic, horizontal mirror fix

## How To Use

In your YAML, point `external_components` to your local `components` folder and select `model: ses42bwr`.

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
    model: ses42bwr
    update_interval: 30s
    lambda: |-
      it.fill(COLOR_OFF);
      it.print(10, 10, id(my_font), COLOR_ON, "SES42BWR");
```

## Notes

- The current implementation mirrors the X axis in software to match the tested SES42BWR panel wiring/orientation.
- `full_update_every` is accepted by the YAML because ESPHome expects it, but this model currently behaves as full refresh only.
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
