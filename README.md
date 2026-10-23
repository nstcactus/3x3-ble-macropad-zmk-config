# 3x3 BLE/USB macropad ZMK config

This is a ZMK config for a 3x3 BLE and USB macropad. It uses a 
[Seeed XIAO nrf52840](https://wiki.seeedstudio.com/XIAO_BLE/) microcontroller and runs on a 300 mAh LiPo battery.

## Wiring

Keyboard origin is at the top-left corner, key side.

| Position      | Wire          | XIAO pin |
|-------------- | ------------- | -------- |
| Top row       | Black         | D0       |
| Middle row    | Grey          | D1       |
| Bottom row    | Red, thinner  | D2       |
| Left column   | Orange        | D3       |
| Middle column | Red, thicker  | D4       |
| Right column  | Brown         | D5       |

The matrix uses ZMK's `col2row` diode direction: each diode's stripedend (cathode) connects toward its row wire.

## Firmware

Built with ZMK v0.3 using GitHub Actions:

- Board: `seeeduino_xiao_ble`
- Shield: `macropad`
- Connectivity: USB and BLE

## Initial test keymap

Viewed from the key side:

```text
1 2 3
4 5 6
7 8 9
```

These bindings send number-row keycodes, not numeric-keypad keycodes.
The resulting characters depend on the host keyboard layout.