# SES42BWR / BLOZI42 ESPHome External Component

An ESPHome `external_components` repository that adds `ses42bwr` and `blozi42` support to the deprecated `waveshare_epaper` platform.

Current scope:

- Black/white only
- Full refresh only
- Horizontal mirror corrected in software
- Compiles on ESP8266

## Verified Panels

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
- Red/BWR plane support
- Upstream ESPHome compatibility guarantees across future releases

## Repository Layout

```text
components/
  waveshare_epaper/
    __init__.py
    display.py
    waveshare_epaper.h
    waveshare_epaper.cpp
    README.md
examples/
  esp8266_ses42bwr_basic.yaml
```

## Install

Point ESPHome to this repository with `external_components`.

Remote use:

```yaml
external_components:
  - source:
      type: git
      url: https://github.com/miwyf/ses42bwr-esphome
      ref: main
    components: [waveshare_epaper]
```

Local development:

```yaml
# Replace the whole external_components block above with this:
external_components:
  - source:
      type: local
      path: ./components
    components: [waveshare_epaper]
```

## Basic Usage

```yaml
spi:
  clk_pin: GPIO14
  mosi_pin: GPIO13

display:
  - platform: waveshare_epaper
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

Use `model: blozi42` for the BLOZI Endor 4.2 panel.

See [examples/esp8266_ses42bwr_basic.yaml](examples/esp8266_ses42bwr_basic.yaml) for a complete minimal example.

## Notes

- This repository overrides the built-in `waveshare_epaper` component rather than introducing a brand new ESPHome platform name.
- `full_update_every` is accepted because ESPHome generates calls for it, but these models currently behave as full refresh only.
- The implementation was derived from a working Arduino SES42BWR driver and adapted into the ESPHome `waveshare_epaper` structure.
