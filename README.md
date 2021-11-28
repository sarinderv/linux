Note: I did this assignment by myself (no partners).

Here are the steps I followed for Assignment #2 and #3:

# Part A
Build and install new version of KVM module with revised `cpuid` and exit handling:

1. changed `vmx.c` and `cpuid.c` (see source code in this repo) added `total_time` and `total_exits` and `total_time[]` and `total_exits[]` logic to the exit handler, and added a `get_reason()` function which ensures that `%ecx` (on input) contains a valid `EXIT_REASON` subleaf (as defined in `vmx.h`).
2. `make -j 8 modules` - build kernel modules
3. `sudo make INSTALL_MOD_STRIP=1 modules_install` - installs the modules
4. `rmmod kvm` and `rmmod kvm_intel` - unload the previous version of the kvm module
5. `modprobe kvm` and `modprobe kvm_intel` - load the new version

# Part B
Build and run a VM:

1. use `virt-manager` to create a simple Ubuntu VM
2. login to the VM and install the `cpuid` package (`sudo apt install cpuid`)
3. execute `cpuid` with the two different leaf values (see output below)

Below is the final output of `cpuid` instructions in the VM.

Screenshot of "first leaf", showing total_exits in eax:
![Screenshot from 2021-11-20 11-01-36](https://user-images.githubusercontent.com/4393945/142738150-3514b008-37f0-40bc-afa7-2242eae16a3a.png)

Screenshot of "second leaf", showing total_time in ebx/ecx:
![Screenshot from 2021-11-20 10-55-14](https://user-images.githubusercontent.com/4393945/142737998-1153d67a-260f-41f3-bf9f-fa2ccba80148.png)

# Part C

For *CPUID 0x4FFFFFFD* and *0x4FFFFFFC*:

3. The frequency of exits definately increases at a higher rate when the VM is "doing work". For example, the following chart shows the number of exits (polled every second) during a period of a few minutes: ![image](https://user-images.githubusercontent.com/4393945/143732351-48355754-729d-4b3c-a7dc-647753096e81.png)

A full boot entails __approximately 1 million exits__.

4. The most frequent exit types (that are actually defined in the SDM) are:
- __EPT Violation__ (code 48): I believe these are caused by VM nested page faults
- __CPUID__ (code 10): I was polling CPUID frequently from the VM, plus the VM may have been doing additional CPU monitoring
- __I/O Instruction__ (code 30): VM executing I/O instruction

The least frequent exit types are __Access to GDTR or IDTR__ (code 46) and __MOV DR__ (code 29), each of which only occurred twice.

Many exit types did not occur at all (zero exit frequency).

Here is a graph depicting the frequency of Exit Types over the period of about 1 hour in the VM:

![image](https://user-images.githubusercontent.com/4393945/143732539-90c4c08b-a6c0-40a8-bc6c-e721c57f5aef.png)
