# README

The [libopencm3 project](http://libopencm3.org/wiki/Main_Page) is a great API for working with the STM32 family of chips.  It's very easy to get up and running with just a makefile, and at the same time it makes dreadful things like USB easy(er).  They've got a decent [collection of examples](https://github.com/libopencm3/libopencm3-examples) and very good [online API documentation](http://libopencm3.github.io/docs/latest/html/).  It's all open and in active development.

What I've done here is to take a single bit of example code (miniblink) and included the entire `libopencm3` library as a Git submodule for you to check out.  It makes a great springboard for starting up your own code.

And it works across the entire STM32 family:

* [STM32F0 template](https://github.com/hexagon5un/stm32f0_libopencm3_template)
* [STM32F1 template](https://github.com/hexagon5un/stm32f1_libopencm3_template)
* [STM32F2 template](https://github.com/hexagon5un/stm32f2_libopencm3_template)
* [STM32F3 template](https://github.com/hexagon5un/stm32f3_libopencm3_template)
* [STM32F4 template](https://github.com/hexagon5un/stm32f4_libopencm3_template)
* [STM32L0 template](https://github.com/hexagon5un/stm32l0_libopencm3_template)
* [STM32L1 template](https://github.com/hexagon5un/stm32l1_libopencm3_template)

I've tested it out on F0, F1, F3, and F4 chips. Let me know if it works for you on any of the others.

## Installation Instructions

1. Clone this repo

2. `git submodule init && git submodule update` to pull down the `libopencm3` library

3. `cd libopencm3 && make && cd ..` to build `libopencm3`

4. `make` should build you the project at that point -- see what happens.

5. You can burn the firmware directly to your chip using `make miniblink.stlink-flash` if you've got an ST-link hooked up.  See `support/libopencm3.rules.mk` for general `make` rules, and `support/libopencm3.target.mk` for project-specific ones.

6. When this fails to blink, check to make sure that your LEDs are hooked up as the code expects: look for GPIOxx and change it.

7. Get hacking.  You've got better things to do than blink lights!


## Toolchain

To get this running, you'll need at least the [arm-none-eabi toolchain](https://launchpad.net/gcc-arm-embedded), and probably the normal raft of compile tools. 

Flashing the chips is supported using [texane]'s [st-link](https://github.com/texane/stlink) software, which supports the native ST programmers in their Discovery and Nucleo kits.  You can also use a [Black Magic Probe](https://github.com/blacksphere/blackmagic), which is essentially a firmware (incidentally using opencm3!) to flash into another ARM chip.  Or you can use JTAG or whatever else you've got. 

And that's it, really.  If you're thinking of developing ST/ARM chips with GCC, this is pretty much the most painless route I know of...

## Details, details, details.

This repo and a few more like it -- one for each supported ST32Fx ARM flavor -- were generated with [a hacky Python script](https://gist.github.com/hexagon5un/c88f505e53f9b8ae64ff). You can modify that to work for other examples from the library if you want, but we warned that they're not all supported on all hardware.

Similarly, you could get these examples running on the NXP, TI, or Atmel ARM chips and then you'd have a cross-platform, cross-vendor starting point.  If you do that, let me (and the `libopencm3`) folks know?


### Avoid Duplicating the Library

Ideally, you'll stop copying over the entire `libopencm3` library for every project.  When you make your second application, you've got two decent options:

1. Move the `libopencm3` directory someplace reasonable on your computer and change the toplevel Makefile accordingly.

2. Move the `libopencm3` directory and then symlink to it from within your code.

Both have pros and cons.  One is a library dependency that's outside of the directory / repo, so other people using your code will have to care about paths and installing it and so on.  The second relies on symlinks -- kinda hacky.

### Linker Scripts

I've put all of the linker scripts from the example code into the `support` subdirectory.  You can see how little they differ from each other -- just the RAM/ROM sizes.  This is a testament to some nice coding by the `libopencm3` folks, IMO.  

If you've got a different chip with different amounts of ROM/RAM, just copy and edit a linker script.  It's really easy.  If you don't know how much ROM/RAM your chip has, try `st-info --flash` and `st-info --sram` with your chip plugged in.  Copy those hex values into the right places in the linker script and you're set. 

### Other Examples

If you start diving deeper into `opencm3`, the API docs are your best friend, and the [example code project](https://github.com/libopencm3/libopencm3-examples) shouldn't be far away.  You can copy the code straight out of the examples, but be warned that I've had to tweak the Makefiles to work with my particular (flat) layout.  Compare and contrast before you overwrite.  

Basically, it's just deleting a mention to "../../Makefile.include" and replacing it with an include of the two sub-makefiles in `support`.  You'll also need to pass it the location of the libopencm3 libraries, of course.



