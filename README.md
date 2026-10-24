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

Built with ZMK v0.3 using GitHub Actions or locally with Docker:

- Board: `seeeduino_xiao_ble`
- Shield: `macropad`
- Connectivity: USB and BLE

### Local build with Docker

Prerequisites: Docker installed and running, with permission to run
`docker run` as your user. Run all commands below from the repository root.

#### First-time setup

Create a separate west workspace for build dependencies and output:

```sh
mkdir -p .local-build/config
cp config/west.yml .local-build/config/west.yml
```

The `.local-build/` directory is ignored by Git. It is separate from the
scaffolding CLI's `.zmk/` workspace.

Initialize and download dependencies:

```sh
docker run --rm \
  --user "$(id -u):$(id -g)" \
  -e HOME=/tmp \
  -v "$PWD:/workspaces/macropad:z" \
  -w /workspaces/macropad/.local-build \
  zmkfirmware/zmk-dev-arm:3.5 \
  bash -c 'west init -l config && west update && west zephyr-export'
```

The initial download can take a while and requires several gigabytes of disk
space. Skip this initialization if `.local-build/` is already initialized and
its dependencies have been downloaded.

The Docker image matches the Zephyr 3.5 toolchain used by ZMK v0.3. The `:z`
mount option allows container access under Fedora's SELinux, and `--user`
keeps generated files owned by your user.

#### Build and rebuild

Use this one-liner for both the first build and subsequent builds:

```sh
docker run --rm --user "$(id -u):$(id -g)" -e HOME=/tmp -v "$PWD:/workspaces/macropad:z" -w /workspaces/macropad/.local-build zmkfirmware/zmk-dev-arm:3.5 west build -p always -s zmk/app -d build/macropad -b seeeduino_xiao_ble -- -DZMK_CONFIG=/workspaces/macropad/config -DZMK_EXTRA_MODULES=/workspaces/macropad -DSHIELD=macropad
```

`-p always` performs a pristine rebuild so configuration changes are applied
reliably. The firmware is written to:

```text
.local-build/build/macropad/zephyr/zmk.uf2
```

If `config/west.yml` changes, copy it into `.local-build/config/west.yml`
again and run `west update` inside the same container environment before
rebuilding. The manifest copy does not update automatically.

## Initial test keymap

Viewed from the key side:

```text
1 2 3
4 5 6
7 8 9
```

These bindings send number-row keycodes, not numeric-keypad keycodes.
The resulting characters depend on the host keyboard layout.