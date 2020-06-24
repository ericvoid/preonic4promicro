# DIY-Preonic + Pro Micro + QMK Firmware

Guide and code to make a QMK firmware for a DIY 5x12 ortholinear keyboard
(aka Preonic) using a Pro Micro board. The firmware is based on `preonic/rev2`
because the rev3 is based on ARM processor.

This repo contains two keymaps:

* `preonic4promicro`: the keymap itself is the same as Preonic, there are
  changes only on `config.h` to use appropriate pins on the board.
* `preonic4promicro-mine`: a custom keymap fitting my personal requisites.

The Pro Micro board has 18 I/O ports, which matches exactly the number of
required pins: 5 row pins, 12 column pins, 1 pin for the speaker (optional).


# Install, Compile and Flash (Ubuntu)

Install Python 3, pip and QMK. Refer to the official guide.

After setting up QMK with `$ qmk setup`, it will create a directory for all
the files, such as `~/qmk_firmware/`.

Clone/download the files in this repo, and copy them to preonic's keymaps
directory:

    $ mkdir -p ~/qmk_firmware/keyboards/preonic/keymaps/preonic4promicro
    $ cp preonic4promicro/* ~/qmk_firmware/keyboards/preonic/keymaps/preonic4promicro/

Compile the firmware:

    $ qmk compile -kb preonic/rev2 -km preonic4promicro

Connect the Pro Micro and flash the firmware:

    $ qmk flash -kb preonic/rev2 -km preonic4promicro -bl avrdude

If you are having issues with flashing, it can be required to uninstall the
Modem Manager package:

    sudo apt-get remove modemmanager


# Customizing for your own hardware setup

You can use any board with ATmega32u4 chip for this project.

To set the pins used for rows and columns of the matrix, change the following
constants in `config.h`:

* `MATRIX_ROW_PINS`: the array of pins used for the rows (top rows first);
* `MATRIX_COL_PINS`: the array of pins used for the columns (left cols first,
  or right cols first when looking the keyboard from behind).

For example, if you use a port called "PD7", set the array possition with "D7".

When building the key matrix, prefer making your diodes face the rows. If your
diodes are facing the rows, there are no need to change the code. Otherwise, if
the diodes are facing the columns, you will have to change the
`DIODE_DIRECTION` constant.

If your matrix has more than 5 rows and 12 columns (or less), you are going
to have to change the `MATRIX_ROWS` and `MATRIX_COLS` constants. The length
of the PINS array above also will have to match the rows and cols count.

You can attach a buzzer (or a small speaker) to C6 and GND. It will work
without any changes.
