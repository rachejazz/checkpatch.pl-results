On the mainline tree, several patches were made with the command:
```
git show 906a3c6f9ca072e917c701f7421647e169740954
git show b220c049d5196dd94d992dd2dc8cba1a5e6123bf

git format-patch -1 906a3c6f9ca072e917c701f7421647e169740954
git format-patch -1 b220c049d5196dd94d992dd2dc8cba1a5e6123bf

0001-io_uring-don-t-acquire-uring_lock-twice.patch
0001-tracing-Check-length-before-giving-out-the-filter-bu.patch
```

Then checkpatch script was run to notice types of warnings
```
./scripts/checkpatch.pl --show-types --codespell --fix 0001-io_uring-don-t-acquire-uring_lock-twice.patch > output1
./scripts/checkpatch.pl --show-types --codespell --fix 0001-tracing-Check-length-before-giving-out-the-filter-bu.patch > output2

cat output*
```
