```yaml
number: 1631
title: AIX support
type: issue
state: closed
author: scottismyname
labels:
  - wontfix
assignees: []
created_at: 2020-06-28T16:08:50Z
updated_at: 2020-06-28T16:23:32Z
url: https://github.com/BurntSushi/ripgrep/issues/1631
synced_at: 2026-01-12T16:13:23Z
```

# AIX support

---

_@scottismyname_

I know this is probably a long shot, but my work uses AIX. I don't have permission to install code on our machine, but was wondering if I could somehow get a binary I could run directly. 

---

_Comment by @BurntSushi on 2020-06-28 16:23_

Unfortunately, I don't know anything about AIX and I'm not inclined to add more targets to the CI matrix unless it's really well motivated.

Your best bet would be to use [cross](https://github.com/rust-embedded/cross), although I [don't see AIX on its list of supported targets](https://github.com/rust-embedded/cross#supported-targets).

I don't see it on [rustc's supported target list either](https://forge.rust-lang.org/release/platform-support.html).

Googling for "llvm AIX support" yields this: https://lists.llvm.org/pipermail/llvm-dev/2019-February/130175.html

So I'd say you're out of luck for now.

---

_Closed by @BurntSushi on 2020-06-28 16:23_

---

_Label `wontfix` added by @BurntSushi on 2020-06-28 16:23_

---
