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

```console
[  151.770522] cmpe283_1: loading out-of-tree module taints kernel.
[  151.770568] cmpe283_1: module verification failed: signature and/or required key missing - tainting kernel
[  151.770857] CMPE 283 Assignment 1 Module Start
[  151.770859] Pinbased Controls MSR: 0x7f00000016
[  151.770862]   External Interrupt Exiting: Can set=Yes, Can clear=Yes
[  151.770863]   NMI Exiting: Can set=Yes, Can clear=Yes
[  151.770865]   Virtual NMIs: Can set=Yes, Can clear=Yes
[  151.770866]   Activate VMX Preemption Timer: Can set=Yes, Can clear=Yes
[  151.770868]   Process Posted Interrupts: Can set=No, Can clear=Yes
[  151.770869] Procbased Controls MSR: 0xfff9fffe0401e172
[  151.770871]   Interrupt-window exiting: Can set=Yes, Can clear=Yes
[  151.770872]   Use TSC offsetting: Can set=Yes, Can clear=Yes
[  151.770873]   HLT exiting: Can set=Yes, Can clear=Yes
[  151.770875]   INVLPG exiting: Can set=Yes, Can clear=Yes
[  151.770876]   MWAIT exiting: Can set=Yes, Can clear=Yes
[  151.770877]   RDPMC exiting: Can set=Yes, Can clear=Yes
[  151.770879]   RDTSC exiting: Can set=Yes, Can clear=Yes
[  151.770880]   CR3-load exiting: Can set=Yes, Can clear=No
[  151.770881]   CR3-store exiting: Can set=Yes, Can clear=No
[  151.770883]   Activate tertiary controls: Can set=No, Can clear=Yes
[  151.770884]   CR8-load exiting: Can set=Yes, Can clear=Yes
[  151.770885]   CR8-store exiting: Can set=Yes, Can clear=Yes
[  151.770887]   Use TPR shadow: Can set=Yes, Can clear=Yes
[  151.770888]   NMI-window exiting: Can set=Yes, Can clear=Yes
[  151.770889]   MOV-DR exiting: Can set=Yes, Can clear=Yes
[  151.770891]   Unconditional I/O exiting: Can set=Yes, Can clear=Yes
[  151.770892]   Use I/O bitmaps: Can set=Yes, Can clear=Yes
[  151.770893]   Monitor trap flag: Can set=Yes, Can clear=Yes
[  151.770895]   Use MSR bitmaps: Can set=Yes, Can clear=Yes
[  151.770896]   MONITOR exiting: Can set=Yes, Can clear=Yes
[  151.770898]   PAUSE exiting: Can set=Yes, Can clear=Yes
[  151.770899]   Activate secondary controls: Can set=Yes, Can clear=Yes
[  151.770900] Secondary Procbased Controls MSR: 0x7f00000000
[  151.770903]   Virtualize APIC accesses: Can set=Yes, Can clear=Yes
[  151.770904]   Enable EPT: Can set=Yes, Can clear=Yes
[  151.770905]   Descriptor-table exiting: Can set=Yes, Can clear=Yes
[  151.770906]   Enable RDTSCP: Can set=Yes, Can clear=Yes
[  151.770908]   Virtualize x2APIC mode: Can set=Yes, Can clear=Yes
[  151.770909]   Enable VPID: Can set=Yes, Can clear=Yes
[  151.770910]   WBINVD exiting: Can set=Yes, Can clear=Yes
[  151.770911]   Unrestricted guest: Can set=No, Can clear=Yes
[  151.770912]   APIC-register virtualization: Can set=No, Can clear=Yes
[  151.770913]   Virtual-interrupt delivery: Can set=No, Can clear=Yes
[  151.770914]   PAUSE-loop exiting: Can set=No, Can clear=Yes
[  151.770915]   RDRAND exiting : Can set=No, Can clear=Yes
[  151.770916]   Enable INVPCID: Can set=No, Can clear=Yes
[  151.770917]   Enable VM functions: Can set=No, Can clear=Yes
[  151.770918]   VMCS shadowing: Can set=No, Can clear=Yes
[  151.770919]   Enable ENCLS exiting: Can set=No, Can clear=Yes
[  151.770920]   RDSEED exiting: Can set=No, Can clear=Yes
[  151.770921]   Enable PML: Can set=No, Can clear=Yes
[  151.770922]   EPT-violation #VE: Can set=No, Can clear=Yes
[  151.770922]   Conceal VMX from PT: Can set=No, Can clear=Yes
[  151.770923]   Enable XSAVES/XRSTORS: Can set=No, Can clear=Yes
[  151.770924]   Mode-based execute control for EPT: Can set=No, Can clear=Yes
[  151.770925]   Sub-page write permissions for EPT: Can set=No, Can clear=Yes
[  151.770926]   Intel PT uses guest physical addresses: Can set=No, Can clear=Yes
[  151.770927]   Use TSC scaling: Can set=No, Can clear=Yes
[  151.770928]   Enable user wait and pause: Can set=No, Can clear=Yes
[  151.770929]   Enable ENCLV exiting: Can set=No, Can clear=Yes
[  151.770930] VM-Exit Controls MSR: 0x7fffff00036dff
[  151.770931]   Save debug controls: Can set=Yes, Can clear=No
[  151.770932]   Host address-space size: Can set=Yes, Can clear=Yes
[  151.770933]   Load IA32_PERF_GLOBAL_CTRL: Can set=Yes, Can clear=Yes
[  151.770934]   Acknowledge interrupt on exit: Can set=Yes, Can clear=Yes
[  151.770935]   Save IA32_PAT: Can set=Yes, Can clear=Yes
[  151.770936]   Load IA32_PAT: Can set=Yes, Can clear=Yes
[  151.770937]   Save IA32_EFER: Can set=Yes, Can clear=Yes
[  151.770938]   Load IA32_EFER: Can set=Yes, Can clear=Yes
[  151.770939]   Save VMX-preemption timer value: Can set=Yes, Can clear=Yes
[  151.770940]   Clear IA32_BNDCFGS: Can set=No, Can clear=Yes
[  151.770941]   Conceal VMX from PT: Can set=No, Can clear=Yes
[  151.770942]   Clear IA32_RTIT_CTL: Can set=No, Can clear=Yes
[  151.770943]   Load CET state: Can set=No, Can clear=Yes
[  151.770944]   Load PKRS: Can set=No, Can clear=Yes
[  151.770945] VM-Entry Controls MSR: 0xffff000011ff
[  151.770947]   Load debug controls: Can set=Yes, Can clear=No
[  151.770949]   IA-32e mode guest: Can set=Yes, Can clear=Yes
[  151.770950]   Entry to SMM: Can set=Yes, Can clear=Yes
[  151.770952]   Deactivate dual-monitor treatment: Can set=Yes, Can clear=Yes
[  151.770953]   Load IA32_PERF_GLOBAL_CTRL: Can set=Yes, Can clear=Yes
[  151.770954]   Load IA32_PAT: Can set=Yes, Can clear=Yes
[  151.770955]   Load IA32_EFER: Can set=Yes, Can clear=Yes
[  151.770957]   Load IA32_BNDCFGS: Can set=No, Can clear=Yes
[  151.770958]   Conceal VMX from PT: Can set=No, Can clear=Yes
[  151.770959]   Load IA32_RTIT_CTL: Can set=No, Can clear=Yes
[  151.770960]   Load CET state: Can set=No, Can clear=Yes
[  151.770962]   Load PKRS: Can set=No, Can clear=Yes
[  151.780995] CMPE 283 Assignment 1 Module Exits
```
