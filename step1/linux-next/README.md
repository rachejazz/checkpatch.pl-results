Some recent commits from the linux-next tree:

```
divya@ubuntu:~/linux-next$ git log -6 --no-merges --pretty=reference
07f7e57c63aa (Add linux-next specific files for 20210212, 2021-02-12)
25730252dd6f (MIPS: make userspace mapping young by default, 2021-02-12)
dda8fe227129 (initramfs-panic-with-memory-information-fix, 2021-02-12)
d09f16d04cd8 (initramfs: panic with memory information, 2021-02-12)
561afcd94f09 (ubsan: remove overflow checks, 2021-02-12)
0a12f9dd9c0b (scripts/gdb: fix list_for_each, 2021-02-12)
```
Following errors and warnings were seen on running script on linux-next tree
```
./scripts/checkpatch.pl --show-types -g `git log -6 --no-merges --oneline | cut -f 1 -d " "` > output-next
cat output-next
```
