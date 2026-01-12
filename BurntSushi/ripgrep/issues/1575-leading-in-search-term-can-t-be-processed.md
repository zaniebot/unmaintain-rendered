```yaml
number: 1575
title: "leading / in search term can't be processed correctly on Windows OS"
type: issue
state: closed
author: jia2
labels:
  - duplicate
  - wontfix
assignees: []
created_at: 2020-05-08T14:50:25Z
updated_at: 2020-05-08T15:19:50Z
url: https://github.com/BurntSushi/ripgrep/issues/1575
synced_at: 2026-01-12T16:13:23Z
```

# leading / in search term can't be processed correctly on Windows OS

---

_@jia2_

This issue is only on Windows OS.

#### What version of ripgrep are you using?
ripgrep 12.0.1 (rev 1d5b1011e5)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?
use Github binary releases, both windows-msvc an windows-gnu versions

#### What operating system are you using ripgrep on?
Windows 10 Version 1903, OS Build 18362.778

#### Describe your bug.

ripgrep doesn't work when search term has a leading "/", for example "/a". 

#### What are the steps to reproduce the behavior?

```bash
echo "/a/b/c/d" >> test.txt
echo "a/b/c/d >> test.txt
rg "/a/" test.txt  # --> nothing found
rg '/a/' test.txt  # --> nothing found
rg -F '/a/' test.txt  # --> nothing found
```

#### What is the actual behavior?
I run it with git bash on windows.

```bash
jia@ (⎈ |wus:cloudia-wus) dev $ rg --debug  '/a/b' test.txt
DEBUG|grep_regex::literal|crates\regex\src\literal.rs:58: literal prefixes detected: Literals { lits: [Complete(A:/b)], limit_size: 250, limit_class: 10 }
DEBUG|globset|crates\globset\src\lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes

```

#### What is the expected behavior?
The first line "/a/b/c/d" shoud be in the search result. 



---

_Comment by @BurntSushi on 2020-05-08 15:19_

Duplicate of #1277

This isn't a problem with ripgrep. It's a problem with your environment. IIRC, your shell is replacing `/` with something else. As far as I'm concerned, this is a broken environment and ripgrep shouldn't do anything to fix it. As a work-around, you can use `[/]a/b/c/d`.

---

_Closed by @BurntSushi on 2020-05-08 15:19_

---

_Label `duplicate` added by @BurntSushi on 2020-05-08 15:19_

---

_Label `wontfix` added by @BurntSushi on 2020-05-08 15:19_

---
