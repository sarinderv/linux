Here are the steps I followed for this assignment:

Part A - build and install new kernel version

0. Forke the torvalds/linux Github repo into my own account, clone it locally into `~/linux`, and cd into that new directory.
1. `uname -a` - gives the current running linux version
2. `cp /boot/config-5.11.0-37-generic .config` - copies the current boot config locally
3. `make oldconfig` - takes the current boot config into the new one (I held down the 'enter' key per the Professor's suggestion to get all the default new values)
4. `make prepare`
5. `make -j 8 modules` - builds all the new kernel modules
6. `make -j 8` - builds the new kernel
7. `sudo make INSTALL_MOD_STRIP=1 modules_install` - installs the modules
8. `sudo make install` - installs the kernel
9. `sudo reboot`

At step 7 I ran into an issue which turned out was caused by step 5 not completing properly. By default, the make config was trying to sign the kernel modules with a Canonical private key, which obviously I don't have. The fix was to manually set `CONFIG_SYSTEM_TRUSTED_KEYS=""` in the .config file (see https://askubuntu.com/a/1329625 and https://wiki.archlinux.org/title/Signed_kernel_modules#Kernel_configuration for more details).

Part B - 

0. create a `cmpe283` directory under `~/linux` and copy the Professor's Makefile and .c file
1. Append a license info at the end of the .c file
2. Fill out the rest of the VMX field checks in the .c file
3. `make`
4. `sudo installmod` - install the module
5. `sudo removemod` - remove the module
6. `dmesg` - obtain the output of the `printk` lines

I ended up adding a new make directive `showmod` with lines 3,4,5 above since I had to run those repeatedly during development.

Below is the final output of `dmesg`:

<TODO: insert output here>
