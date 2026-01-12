```yaml
number: 784
title: rg --files does not ignore ELF/ARM binaries
type: issue
state: closed
author: sidalit
labels:
  - question
  - wontfix
assignees: []
created_at: 2018-02-09T20:29:34Z
updated_at: 2018-02-09T21:16:57Z
url: https://github.com/BurntSushi/ripgrep/issues/784
synced_at: 2026-01-12T16:13:22Z
```

# rg --files does not ignore ELF/ARM binaries

---

_@sidalit_

#### What version of ripgrep are you using?

ripgrep 0.7.1
-AVX -SIMD

#### What operating system are you using ripgrep on?

Ubuntu 16.04.3 LTS

#### Describe your question, feature request, or bug.

I found that rg --files output ARM/ELF files even if it is supposed to ignore binaries. I work a lot with cross-compilation binaries (buildroot and yocto) and in my case, ignoring ARM/ELF files is useful.

Do you have this feature planned ?

Thanks !


---

_Comment by @BurntSushi on 2018-02-09 21:15_

Binary files can only be detected by searching them (a file is treated as binary if it contains a `NUL` byte), and the `--files` flag specifically does not search files. Therefore, it cannot rule out binary files.

If you need this and can't get it any other way, then you might consider using `-l/--files-with-matches` instead along with a trivial pattern. It isn't foolproof since with a trivial pattern it will likely only look at the beginning block of a file. So if that block doesn't contain a `NUL` byte, then it will be treated as binary.

---

_Closed by @BurntSushi on 2018-02-09 21:15_

---

_Label `question` added by @BurntSushi on 2018-02-09 21:15_

---

_Label `wontfix` added by @BurntSushi on 2018-02-09 21:15_

---
