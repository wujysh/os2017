# Operating System Labs 2017 for Fudan University

## Install the patched QEMU emulator
QEMU is a modern and fast PC emulator.

Unfortunately, QEMU's debugging facilities, while powerful, are somewhat immature, so we highly recommend you use the patched version of QEMU instead of the stock version that may come with your distribution.

  1. Clone the IAP 6.828 QEMU git repository. `git clone http://web.mit.edu/ccutler/www/qemu.git -b 6.828-2.3.0`
  2. You may need to install several libraries.
        - Ubuntu 16.04: `sudo apt install libsdl1.2-dev libtool-bin libglib2.0-dev libz3-dev libpixman-1-dev`
        - Ubuntu 12.04: `sudo apt-get install libsdl1.2-dev libtool libglib2.0-dev libpixman-1-dev`
  3. Configure the source code. `./configure --disable-kvm --target-list="i386-softmmu x86_64-softmmu"`
  4. Run `make` to compile QEMU.
  5. Run `sudo make install` to install QEMU.

## References
- [Lab tools guide](https://pdos.csail.mit.edu/6.828/2016/labguide.html)
- [Reading materials](https://pdos.csail.mit.edu/6.828/2016/reference.html)
