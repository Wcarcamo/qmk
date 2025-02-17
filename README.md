# QMK

I bought a pre-built [YMDK Split 75% 84 Keyboard](https://ymdkey.com/products/ymdk-split-75-84-acrylic-case-plate-underglow-rgb-fully-programmable-pcb-stabilizers-ansi-iso-diy-kit-similar-to-vea-layout?_pos=6&_sid=bdc177401&_ss=r) off facebook marketplace.

I wanted to experience what it's like with a split keyboard and using layers.

To remap the keyboard, I used [QMK](https://qmk.fm/) instead of the default [BootMapper Client](https://drive.google.com/file/d/1vMUbjzyKlsihNI8jNVvvqAk7vpWzmBqr/view) that the vender recommends. 
I run a linux machine and this software runs only on Windows & MacOS (also found it odd that official link points to a google drive folder).

# Instructions

1. Install QMK

```bash
cd <project directory>

# Create Python Virtual Environment
python3 -m venv .venv

# Activate Virtual Environment
source .venv/bin/activate

# Install QMK
python3 -m pip install qmk
```

> 🗒️ **NOTE**: Once installed, all `QMK` CLI commands in the following steps must be ran from an activated python virtual environment.



2. Set up QMK

> 🗒️ **TIP**: During setup, QMK cloned the `qmk_firmware` repository into my home directory. You can use `qmk setup -H <path>` if you want to use another location. 

```bash
# Set Up QMK
qmk setup
```

3. Configure QMK Build Environment

> 🗒️ **NOTE**: The YMDK 75% 84 is not a listed supported keyboard by QMK. The closest keyboard I've found is **mt/split75**. 

```bash
# Configure Build Defaults
qmk config user.keyboard=mt/split75
qmk config user.keymap=<Custom keymap name>
```

4. Create a new `C` keymap: `keymap.c`
    - The `keymap.c` file is located in the `qmk_firmware` repository based on what you configured in the previous step, in my case: `~/qmk_firmware/keyboards/mt/split75/keymaps/wcarcamo/keymap.c`

```bash
qmk new-keymap
```

5. If you don't want to manually edit your `C` keymap, I recommend using [QMK Configurator](https://config.qmk.fm/) to create a `JSON` keymap instead:
    - Keyboard: mt/split75
    - Layout: LAYOUT
    - Keymap Name: <Custom keymap name>

6. Download your `JSON` Keymap: `keymap.json`

7. Use QMK to convert your `JSON` keymap into a `C` keymap

```bash
qmk json2c <path to keymap.json>
```

6. Update your `keymap.c` with the printed output

7. Compile your keyboard with the latest keymap
    - Use `-c` flag for a clean compile

```bash
# If you configured defaults, step 3
qmk compile

# If you did not configure defaults
qmk compile -kb <keyboard> -km <keymap>
```

8. Put Your Keyboard into DFU (Bootloader) Mode
    - For the YMDK 75% 84 keyboard, hold the left control key while you plug in your keyboard
    - The caps lock LED will flash on and off indicating it is ready to be flashed

9. Flash your keyboard with the latest firmware containing your custom keymap

```bash
qmk flash
```

10. Once complete (LED stops flashing), your keyboard is ready for use