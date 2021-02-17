Next, we will take 3 WARNINGs and study the purpose of each

## Separate WARNING with commit hashes
Tried a bit different this time. Now I got all the non-merged commits from versions 5.10 to 5.11. Like this:

```
git log --no-merges --pretty=reference v5.10..v5.11 > gitlog
cat file | cut -f 1 -d " " > gitlog_hash
```

Now we run the script on this file containing the hashes. And ONLY grep down the warnings to another file.

```
cat gitlog_hash | while read line; do ./scripts/checkpatch.pl --show-types --terse -g $line >> script_log; done;
cat script_log | grep "WARNING" > script_warn
```

Taking any 3 of these, we move on analyzing them:

## WARNING analysis

#### COMMIT_LOG_LONG_LINE warnings
These are given to prevent overdescriptive or repeatative worded commits. Although weirdly enough, these warnings capture the ones where the line Link: is mentioned.
`--fix` does not fix these errors.
```
cat script_warn | grep "COMMIT_LOG_LONG_LINE"
ade9679c159d5bbe14fb7e59e97daf6062872e2b:9: WARNING:COMMIT_LOG_LONG_LINE: Possible unwrapped commit description (prefer a maximum 75 chars per line)
93ca696376dd3d44b9e5eae835ffbc84772023ec:10: WARNING:COMMIT_LOG_LONG_LINE: Possible unwrapped commit description (prefer a maximum 75 chars per line)
b220c049d5196dd94d992dd2dc8cba1a5e6123bf:13: WARNING:COMMIT_LOG_LONG_LINE: Possible unwrapped commit description (prefer a maximum 75 chars per line)
3286222fc609dea27bd16ac02c55d3f1c3190063:34: WARNING:COMMIT_LOG_LONG_LINE: Possible unwrapped commit description (prefer a maximum 75 chars per line)
28dc10eb77a2db7681b08e3b109764bbe469e347:9: WARNING:COMMIT_LOG_LONG_LINE: Possible unwrapped commit description (prefer a maximum 75 chars per line)
e812cbbbbbb15adbbbee176baa1e8bda53059bf0:6: WARNING:COMMIT_LOG_LONG_LINE: Possible unwrapped commit description (prefer a maximum 75 chars per line)
e88b2c6e5a4d9ce30d75391e4d950da74bb2bd90:29: WARNING:COMMIT_LOG_LONG_LINE: Possible unwrapped commit description (prefer a maximum 75 chars per line)
fd675184fc7abfd1e1c52d23e8e900676b5a1c1a:16: WARNING:COMMIT_LOG_LONG_LINE: Possible unwrapped commit description (prefer a maximum 75 chars per line)
ee114dd64c0071500345439fc79dd5e0f9d106ed:6: WARNING:COMMIT_LOG_LONG_LINE: Possible unwrapped commit description (prefer a maximum 75 chars per line)
2ade0d60939bcd54197c133b03b460fe62a4ec47:8: WARNING:COMMIT_LOG_LONG_LINE: Possible unwrapped commit description (prefer a maximum 75 chars per line)
3d0bc44d39bca615b72637e340317b7899b7f911:10: WARNING:COMMIT_LOG_LONG_LINE: Possible unwrapped commit description (prefer a maximum 75 chars per line)
816ef8d7a2c4182e19bc06ab65751cb9e3951e94:9: WARNING:COMMIT_LOG_LONG_LINE: Possible unwrapped commit description (prefer a maximum 75 chars per line)
:
```

#### LONG_LINE warnings
These look into the file structure and raise a warning when it sees there are more columns(each character as one column) than the limit. Weird thing in this is that even though the patch contains the code line broken into more than one ( judging by my vim giving out line numbers for each small segment of the code line )
```
cat script_warn | grep ":LONG_LINE"
4f6ec8602341e97b364e4e0d41a1ed08148f5e98:37: WARNING:LONG_LINE_COMMENT: line length of 127 exceeds 100 columns
58180a0cc0c57fe62a799a112f95b60f6935bd96:43: WARNING:LONG_LINE: line length of 102 exceeds 100 columns
882554042d138dbc6fb1a43017d0b9c3b38ee5f5:47: WARNING:LONG_LINE: line length of 102 exceeds 100 columns
882554042d138dbc6fb1a43017d0b9c3b38ee5f5:48: WARNING:LONG_LINE: line length of 104 exceeds 100 columns
ba839b7598440a5d78550a115bac21b08d57cc32:90: WARNING:LONG_LINE: line length of 109 exceeds 100 columns
ba839b7598440a5d78550a115bac21b08d57cc32:92: WARNING:LONG_LINE: line length of 109 exceeds 100 columns
ef357e02b6c420dc2d668ebf3165838c77358acd:109: WARNING:LONG_LINE: line length of 101 exceeds 100 columns
d3921cb8be29ce5668c64e23ffdaeec5f8c69399:178: WARNING:LONG_LINE: line length of 114 exceeds 100 columns
004b8ae9e2de55ca7857ba8471209dd3179e088c:34: WARNING:LONG_LINE: line length of 103 exceeds 100 columns
5c02406428d5219c367c5f53457698c58bc5f917:129: WARNING:LONG_LINE: line length of 152 exceeds 100 columns
8bc3d461d0a95bbcc2a0a908bbadc87e198a86a8:43: WARNING:LONG_LINE: line length of 125 exceeds 100 columns
348fe1ca5ccdca0f8c285e2ab99004fdcd531430:47: WARNING:LONG_LINE: line length of 108 exceeds 100 columns
:
```

#### EMBEDDED_FILENAME warnings
These are raised when there are mentions of the filename in the file (even as comments). The problem with these being if anyday the file locations change, the comment in this file also needs to be edited which can be tedious. A simple truncating of these comments or a variable specified just to get the file path can be helpful.
```
cat script_warn | grep "EMBEDDED_FILENAME"
ba6dfce47c4d002d96cd02a304132fca76981172:94: WARNING:EMBEDDED_FILENAME: It's generally not useful to have the filename in the file
d8f6e5c6c83737cfdad46077e614885a3db9e809:40: WARNING:EMBEDDED_FILENAME: It's generally not useful to have the filename in the file
d8f6e5c6c83737cfdad46077e614885a3db9e809:42: WARNING:EMBEDDED_FILENAME: It's generally not useful to have the filename in the file
941ba122ca56756aad82db21d28f283ad33b8dee:85: WARNING:EMBEDDED_FILENAME: It's generally not useful to have the filename in the file
```

WARNINGs hence explained.
Check for the `script_error` file for ERRORs between v5.10 to v5.11 commits.

End of step3
