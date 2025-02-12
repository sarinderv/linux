Answers to Assignment 4 questions:

1. I did the assignment by myself (no partner)

2. I used the `cpuid` package in the VM to get the actual exit counts i.e. `cpuid -l 0x4ffffffd -s $i` where `$i` is a value from 0..74 (from [vmx.h](https://github.com/torvalds/linux/blob/152d32aa846835987966fd20ee1143b0e05036a0/arch/x86/include/uapi/asm/vmx.h#L95)).

Here is an example:

![image](https://user-images.githubusercontent.com/4393945/144972496-afa3ec29-ebe3-4f7f-9d6a-9feb5a93d6ff.png)

*Actually, instead of `bash`, I wrote a small `Perl` script so I could regex out the `eax` value and convert from hex and output to a CSV file and import the data into Excel.*

3. The total exit count was an order of magnitude worse without ept (~1.2M in ept vs ~12M in non-ept). In other words, shadow paging causes much more VM exits compared to nested paging. This is as expected, because with EPT/Nested pages, there are no VM exits because of page faults, INVLPG, or CR3 accesses. But with Shadow paging, every CR3 access (writes and reads) results in an exit. There are ~9.6 million CR3/4 exits just in the VM bootup! Secondly, INVPLG exits are enabled so the VMM can flush the shadow page table mapping to handle each TLB flush from the VM O/S.

4. In the no-ept run, the number of CR_ACCESS exits increase significantly due to the CR3 exits, and that was the bulk of the increase in total exits. There also was a significant increase in the number of INVPLG exits because INVPLG exits are enabled in non-ept mode. The other change was that there are no EPT_VIOLATION exits in the non-ept run because, obviously, EPT is not used. *One change that I cannot explain is the number of NMI exits increase significantly (from ~20k in ept-mode to ~2.4m in non-ept) - my guess is the system is running slower (boot takes longer) and hence there is more time for NMI to occur. There maybe a hardware-related issue with the memory on the PC that I used for these assignments.*

Below is a graph depicting the breakdown of VM exits between the two runs:
![image](https://user-images.githubusercontent.com/4393945/144970219-4eabdcea-8e97-4ea9-9a5c-6f9be6131998.png)
