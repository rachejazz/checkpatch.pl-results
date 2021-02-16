Next, we will take 3 WARNINGs and study the purpose of each

## Separate WARNING with commit hashes
Tried a bit different this time. Now I got all the non-merged commits from versions 5.10 to 5.11. Like this:

```
git log --no-merges --pretty=reference v5.10..v5.11 > gitlog
cat file | cut -f 1 -d " " > gitlog_hash
```

Now we run the script on this file containing the hashes. And ONLY grep down the warnings to another file.

```
./scripts/checkpatch.pl --show-types --terse -g `cat gitlog_hash` > script_log
cat script_log | grep "WARNING" > script_warn
```

Taking any 3 of these, we move on analyzing them:

## WARNING analysis
