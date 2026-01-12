```yaml
number: 1511
title: Ripgrep gets killed by the OS when searching in huge file
type: issue
state: closed
author: johny65
labels:
  - invalid
assignees: []
created_at: 2020-03-09T10:38:46Z
updated_at: 2020-03-09T12:34:29Z
url: https://github.com/BurntSushi/ripgrep/issues/1511
synced_at: 2026-01-12T16:13:23Z
```

# Ripgrep gets killed by the OS when searching in huge file

---

_@johny65_

#### What version of ripgrep are you using?

ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

I downloaded a Github binary release.

#### What operating system are you using ripgrep on?

Ubuntu 19.10.

#### Describe your question, feature request, or bug.

When searching in a huge file (not so huge, ~10 GB), ripgrep consumes all my RAM memory (32 GB) and gets killed by the OS.

#### If this is a bug, what are the steps to reproduce the behavior?

I don't know if this is a bug. I do `rp -f huge_file -F "#400:5"`. The file is a JSON file.


---

_Comment by @johny65 on 2020-03-09 11:00_

Sorry, I misunderstand the `-f` command line option. It works fine.

---

_Closed by @johny65 on 2020-03-09 11:00_

---

_Label `invalid` added by @BurntSushi on 2020-03-09 12:34_

---
