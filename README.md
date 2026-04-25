# SES42 / SES42BWR / BLOZI42 / BLOZI42BWR / SES74BWR ESPHome External Component

An ESPHome `external_components` repository that adds `ses42`, `ses42bwr`, `blozi42`, `blozi42bwr` and `ses74bwr` support to the deprecated `waveshare_epaper` platform.

Current scope:

- `ses42` & `blozi42`: Black/white only, full refresh only
- `ses42bwr` & `blozi42bwr`: Black/white/red (three-color), full refresh only
- `ses74bwr`: Black/white/red (three-color) 7.4", full refresh only
- Horizontal mirror corrected in software
- Compiles on ESP8266

## Verified Panels

Tested with:

- Brand: SES-imagotag / VUSION
- Size: 4.2"
- Type: BWR panel
- Product code: `R42A01101`
- FPC/screen cable marking: `A1360071-00`
- Model names in YAML: `ses42` (mono) or `ses42bwr` (BWR rendering path)

Also adapted for:

- Brand: BLOZI
- Model: Endor
- Screen/FPC marking: `P420016-MF1-A`
- Model names in YAML: `blozi42` (mono) or `blozi42bwr` (BWR rendering path)

Also adapted for:

- Brand: SES-imagotag / VUSION
- Size: 7.4"
- Type: BWR panel
- Controller: Pervasive E2741CS0Bx
- Resolution: 800 × 480
- Model name in YAML: `ses74bwr` (BWR rendering path)

Not included:

- Partial refresh
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

Example for `ses42` (mono rendering):

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
    model: ses42
    update_interval: 30s
    lambda: |-
      it.fill(COLOR_OFF);
      it.print(10, 10, id(my_font), COLOR_ON, "SES42");
```

Example for `ses42bwr` (BWR rendering path):

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

Example for `blozi42bwr` (BLOZI Endor with BWR rendering path):

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
    model: blozi42bwr
    update_interval: 30s
    lambda: |-
      it.fill(COLOR_OFF);
      it.print(10, 10, id(my_font), COLOR_ON, "BLOZI42BWR");
```

Use `model: blozi42` for mono rendering or `model: blozi42bwr` for BWR rendering path on the BLOZI Endor 4.2 panel.

See [examples/esp8266_ses42bwr_basic.yaml](examples/esp8266_ses42bwr_basic.yaml) for a complete minimal example.

Example for `ses74bwr` (SES 7.4" BWR rendering path):

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
    model: ses74bwr
    update_interval: 30s
    lambda: |-
      it.fill(COLOR_OFF);
      it.print(10, 10, id(my_font), COLOR_ON, "SES74BWR");
```

Use `model: ses74bwr` for BWR rendering on the SES 7.4" panel (800 × 480, Pervasive E2741CS0Bx controller).

## Notes

- This repository overrides the built-in `waveshare_epaper` component rather than introducing a brand new ESPHome platform name.
- Five model variants are available:
  - `ses42`: Legacy mono rendering path for SES 4.2 panels
  - `ses42bwr`: Z21-based black/white/red rendering path for SES 4.2 panels
  - `blozi42`: Mono rendering path for BLOZI Endor 4.2 panels
  - `blozi42bwr`: BWR rendering path for BLOZI Endor 4.2 panels
  - `ses74bwr`: BWR rendering path for SES 7.4" panels (800 × 480, Pervasive E2741CS0Bx)
- `full_update_every` is accepted because ESPHome generates calls for it, but these models currently behave as full refresh only.
- The implementation was derived from a working Arduino SES42BWR driver and adapted into the ESPHome `waveshare_epaper` structure.
