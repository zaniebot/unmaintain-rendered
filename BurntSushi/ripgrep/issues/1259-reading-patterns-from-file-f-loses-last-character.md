```yaml
number: 1259
title: Reading patterns from file (-f) loses last character
type: issue
state: closed
author: dprobinson
labels:
  - bug
assignees: []
created_at: 2019-04-19T10:52:42Z
updated_at: 2019-04-19T11:28:53Z
url: https://github.com/BurntSushi/ripgrep/issues/1259
synced_at: 2026-01-12T16:13:23Z
```

# Reading patterns from file (-f) loses last character

---

_@dprobinson_

#### What version of ripgrep are you using?

ripgrep 11.0.1 (rev a6222939f9)
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)

#### How did you install ripgrep?

Compiled from source

#### What operating system are you using ripgrep on?

Arch Linux

#### Describe your question, feature request, or bug.

When reading patterns in from a file using the "-f" flag, it appears that the last character is lost. Depending on the last character in the patterns file, this causes either a parsing error, or the silent use of a truncated pattern.

#### If this is a bug, what are the steps to reproduce the behavior?

Create the patterns file (patterns.txt):
```
THIS
is
a
test
[broken]
```
N.B. Ensure a blank newline is not added at the end of the file by your editor (see bug #783)

Run the following command (you can use any file as the corpus, as we don't get as far as loading it):
```
rg -P -f patterns.txt testfile
```

#### If this is a bug, what is the actual behavior?

ripgrep reports that there's a parsing error, indicating that the last char "]" has been lost whilst loading the patterns from the file:
```
PCRE2: error compiling pattern at offset 22: missing terminating ] for character class
```
If you modify the patterns.txt file to the following, you no longer receive an error, even though the pattern is now invalid, leading me to believe that the last char is discarded:

```
THIS
is
a
test
[broken]]
```

#### If this is a bug, what is the expected behavior?

Read the last character of the file when loading patterns using "-f", omitting only a trailing newline, as per #783


---

_Label `bug` added by @BurntSushi on 2019-04-19 11:04_

---

_Comment by @BurntSushi on 2019-04-19 11:12_

Fixed in https://github.com/BurntSushi/ripgrep/commit/e7829c05d3b9234f053e7d480f7e75028831d772

---

_Closed by @BurntSushi on 2019-04-19 11:12_

---

_Comment by @BurntSushi on 2019-04-19 11:13_

Thanks for reporting this! I don't think this was a regression in the latest release, so I don't know whether I'll cut a new patch release just for this or not. In the mean time, you can add a `\n` as a work-around. But this should be fixed on master now!

---

_Comment by @dprobinson on 2019-04-19 11:28_

Just recompiled from master, and I can confirm it appears to be fixed in https://github.com/BurntSushi/ripgrep/commit/e7829c05d3b9234f053e7d480f7e75028831d772.

Thanks for the ridiculously fast fix, and for writing such a great tool!

---
