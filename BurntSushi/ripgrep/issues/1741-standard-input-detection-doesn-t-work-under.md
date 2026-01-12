```yaml
number: 1741
title: "Standard input detection doesn't work under PowerShell on macOS"
type: issue
state: closed
author: drrlvn
labels:
  - wontfix
assignees: []
created_at: 2020-11-22T19:19:49Z
updated_at: 2020-11-23T15:24:26Z
url: https://github.com/BurntSushi/ripgrep/issues/1741
synced_at: 2026-01-12T16:13:24Z
```

# Standard input detection doesn't work under PowerShell on macOS

---

_@drrlvn_

#### What version of ripgrep are you using?

```
ripgrep 12.1.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

`brew install ripgrep`

#### What operating system are you using ripgrep on?

macOS Big Sur 11.0.1 (20B29)

#### Describe your bug.

Under PowerShell, when piping output to ripgrep it seems to search in the current directory instead of in the piped input.

#### What are the steps to reproduce the behavior?

In a directory containing a single file `test.txt` containing the line "infile", try searching input using `rg`.

#### What is the actual behavior?

```
$ echo "input" | rg input

$ echo "input" | rg input -
input

$ echo "input" | rg infile
test.txt
1:infile
```

#### What is the expected behavior?

The input should be searched instead of current directory.


---

_Comment by @r-darwish on 2020-11-22 19:32_

In addition, I checked Ubuntu 18.04 and Windows 10 with the same version of Powershell. Windows works as expected and Ubuntu has the same problem.

---

_Comment by @drrlvn on 2020-11-22 19:39_

Forgot to mention my PowerShell version:
```
PowerShell 7.1.0
```

---

_Label `wontfix` added by @BurntSushi on 2020-11-22 20:09_

---

_Comment by @BurntSushi on 2020-11-22 20:11_

This is unlikely a bug in ripgrep, and likely an issue with PowerShell.

If there's an easy fix that ripgrep (or more likely, the atty crate) can do, then folks are welcome to do the research and propose a fix. But I have no plans to do that.

---

_Closed by @BurntSushi on 2020-11-22 20:11_

---

_Reopened by @BurntSushi on 2020-11-23 15:20_

---

_Closed by @BurntSushi on 2020-11-23 15:24_

---
