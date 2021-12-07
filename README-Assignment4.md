Answers to Assignment 4 questions:

1. I did the assignment by myself (no partner)
2. I used the `cpuid` package in the VM to get the actual exit counts i.e. `cpuid -l 0x4ffffffd -s $i`
3. The total exit count was an order of magnitude worse without ept. In other words, shadow paging causes much more VM exits compared to nested paging. This is as expected, because with EPT/Nested pages, there are no VM exits because of page faults, INVLPG, or CR3 accesses. But with Shadow paging, every CR3 access (writes and reads) results in an exit. Secondly, (on every process context switch) the shadow page is initially empty and, furthermore, page fault exits are enabled too. Lastly, INVPLG exits are enabled so the VMM can flush the shadow page table mapping to handle the correspondong TLB flush that the VM O/S did.
4. In the no-ept run, the number of CR_ACCESS exits increase significantly and that was the bulk of the increase in total exits. There also was a significant increase in the number of INVPLG exits.

Below is a graph depicting the breakdown of VM exits between the two runs:
