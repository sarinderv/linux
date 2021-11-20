Note: I did this assignment by myself (no partners).

Here are the steps I followed for Assignment #2:

# Part A
Build and install new version of KVM module with revised `cpuid` and exit handling:

1. changed `vmx.c` and `cpuid.c` (see source code in this repo) added `total_time` and `total_exits` logic to the exit handler
2. `make -j 8 modules` - build kernel modules
3. `sudo make INSTALL_MOD_STRIP=1 modules_install` - installs the modules
4. `rmmod kvm` and `rmmod kvm_intel` - unload the previous version of the kvm module
5. `modprobe kvm` and `modprobe kvm_intel` - load the new version

# Part B
Build and run a VM:

1. use `virt-manager` to create a simple Ubuntu VM
2. login to the VM and install the `cpuid` package (`sudo apt install cpuid`)
3. excute `cpuid` with the two different leaf values (see output below)

Below is the final output of `cpuid` instructions in the VM.

Screenshot of "second leaf", showing total_time in ebx/ecx:
![Screenshot from 2021-11-20 10-55-14](https://user-images.githubusercontent.com/4393945/142737998-1153d67a-260f-41f3-bf9f-fa2ccba80148.png)

