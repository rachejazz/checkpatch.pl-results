## Listing Non-merge commits before merge window

Next, all the errors and warnings of all "non merged" commits of v5-rc6 to v5-rc7 is taken
(since v5.10..v5.11-rc7) git log was too large to process, I continued with 259 commits of rc6 and rc7

```
divya@ubuntu:~/linux-mainline$ git log --no-merges --pretty=reference v5.11-rc6..v5.11-rc7
92bf22614b21 (Linux 5.11-rc7, 2021-02-07)
    816ef8d7a2c4 (x86/efi: Remove EFI PGD build time checks, 2021-02-05)
    36a6c843fd0d (entry: Use different define for selector variable in SUD, 2021-02-05)
    6342adcaa683 (entry: Ensure trap after single-step on system call return, 2021-02-03)
    2452483d9546 (Revert "lib: Restrict cpumask_local_spread to houskeeping CPUs", 2021-02-05)
    4c7bcb51ae25 (genirq: Prevent [devm_]irq_alloc_desc from returning irq 0, 2020-12-21)
    21b200d09182 (cifs: report error instead of invalid when revalidating a dentry fails, 2021-02-05)
    3943abf2dbfa (x86/debug: Prevent data breakpoints on cpu_dr7, 2021-02-04)
    c4bed4b96918 (x86/debug: Prevent data breakpoints on __per_cpu_offset, 2021-02-04)
    654eb3f2a009 (MAINTAINERS/.mailmap: use my @kernel.org address, 2021-02-04)
    e558464be982 (mm: hugetlb: fix missing put_page in gather_surplus_pages(), 2021-02-04)
    28abcc963149 (ubsan: implement __ubsan_handle_alignment_assumption, 2021-02-04)
    b99acdcbfe3c (kasan: make addr_has_metadata() return true for valid addresses, 2021-02-04)
    49c6631d3b4f (kasan: add explicit preconditions to kasan_report(), 2021-02-04)
    da74240eb3fc (mm/filemap: add missing mem_cgroup_uncharge() to __add_to_page_cache_locked(), 2021-02-04)
    9c41e526a56f (mailmap: add entries for Manivannan Sadhasivam, 2021-02-04)
    4c415b9a710b (mailmap: fix name/email for Viresh Kumar, 2021-02-04)
    2dcb39645441 (memblock: do not start bottom-up allocations with kernel_end, 2021-02-04)
    1c2f67308af4 (mm: thp: fix MADV_REMOVE deadlock on shmem THP, 2021-02-04)
    55b6f763d8bc (init/gcov: allow CONFIG_CONSTRUCTORS on UML to fix module gcov, 2021-02-04)
    4f6ec8602341 (mm/vmalloc: separate put pages and flush VM flags, 2021-02-04)
    74e21484e40b (mm, compaction: move high_pfn to the for loop scope, 2021-02-04)
    71a64f618be9 (mm: migrate: do not migrate HugeTLB page whose refcount is one, 2021-02-04)
    ecbf4724e606 (mm: hugetlb: remove VM_BUG_ON_PAGE from page_huge_active, 2021-02-04)
    0eb2df2b5629 (mm: hugetlb: fix a race between isolating and freeing page, 2021-02-04)
    7ffddd499ba6 (mm: hugetlb: fix a race between freeing and dissolving the page, 2021-02-04)
    585fc0d2871c (mm: hugetlbfs: fix cannot migrate the fallocated HugeTLB page, 2021-02-04)
    24c242ec7abb (ntp: Use freezable workqueue for RTC synchronization, 2021-01-25)
    b35ccebe3ef7 (vdpa/mlx5: Restore the hardware used index after change map, 2021-02-04)
:
```

Number of commits taken for statistical analysis:
```
divya@ubuntu:~/linux-mainline$ git log --no-merges --pretty=reference v5.11-rc6..v5.11-rc7 | wc -l
259
```

## Analyzing errors vs warning

Next, I have written a small script that will feed all the above commits one at a time to checkpatch.pl script

```
/scripts/checkpatch.pl --show-types -g `git log --no-merges --pretty=reference v5.11-rc6..v5.11-rc7 | cut -f 1 -d " "`
```

My pc crashed :( 
Printing the error/warning with the error/warning type could be hazardous. Lesson: use --terse to only show the error/warning only
Also, with this another script to narrow down results to only the warnings/errors, discarding counts of warnings and errors
And save it to a file named `stats1`

```
./scripts/checkpatch.pl --show-types --terse -g `git log -7 --no-merges --pretty=reference | cut -f 1 -d " "` | cut -f 2 -d " " | tr -d [:digit:] > stats1
cat stats1
```

### Narrow down errors and warning to separate files
Now, another small script to separate all the warnings and errors into 2 separate files: `WARNING.log` and `ERROR.log`

```
cat stats1 | grep -e 'WARNING' > WARNING.log
cat stats1 | grep -e 'ERROR' > ERROR.log
```

End of step2
