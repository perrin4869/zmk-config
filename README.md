## ZMK Configuration for Handwired, Wireless Skeletyl
This repository contains custom ZMK configuration for handwired [Skeletyl](https://github.com/Bastardkb/Skeletyl) build. The build log for the entire keyboard itself can be found [here](https://elescia.wordpress.com/2022/02/27/handwired-wireless-skeletyl/).

Do note that the keymap file under `boards/shield/skeletyl/` is the *default* keymap, while the one under `config/` is the one that will be compiled if it exists. It is heavily advised to leave the default keymap as it is and make your custom changes in the files under the `config/` directory.

## Local Development

Create a zmk-workspace and setup a python venv and install `west`:

```
mkdir ~/zmk-workspace
cd ~/zmk-workspace
python3 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install west
```

Setup the zephyr workspace:

```
west init -m https://github.com/perrin4869/zmk-config --mf config/west.yml
west update
west zephyr-export
pip install -r ~/zmk-workspace/zephyr/scripts/requirements-base.txt
```

Install the [Zephyr SDK](https://docs.zephyrproject.org/3.5.0/develop/getting_started/index.html#install-zephyr-sdk).

Build the firmware:

```
west build -p -d build/left -s zmk/app -b nice_nano_v2 -- -DSHIELD="skeletyl_left" -DZMK_CONFIG=$HOME/zmk-config/config -DZMK_EXTRA_MODULES=$HOME/zmk-config
west build -p -d build/right -s zmk/app -b nice_nano_v2 -- -DSHIELD="skeletyl_right" -DZMK_CONFIG=$HOME/zmk-config/config -DZMK_EXTRA_MODULES=$HOME/zmk-config
west build -p -d build/reset -s zmk/app -b nice_nano_v2 -- -DSHIELD="settings_reset" -DZMK_CONFIG=$HOME/zmk-config/config -DZMK_EXTRA_MODULES=$HOME/zmk-config
```

The firmware is generated in `build/left/zephyr/zmk.uf2`, `build/right/zephyr/zmk.uf2` and `build/reset/zephyr/zmk.uf2`.
