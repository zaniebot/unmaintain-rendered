```yaml
number: 1041
title: This software on the Arch Linux repository does not support the pcre2
type: issue
state: closed
author: yuzumx
labels: []
assignees: []
created_at: 2018-09-06T14:29:14Z
updated_at: 2018-09-06T22:33:47Z
url: https://github.com/BurntSushi/ripgrep/issues/1041
synced_at: 2026-01-12T16:13:22Z
```

# This software on the Arch Linux repository does not support the pcre2

---

_@yuzumx_

#### What version of ripgrep are you using?

ripgrep 0.9.0
-SIMD -AVX

#### How did you install ripgrep?

In Arch Linux Repository:
```sh
pacman -S ripgrep
```

#### What operating system are you using ripgrep on?

Arch Linux @ Linux-4.18.5-arch1-1-ARCH

#### Describe your question, feature request, or bug.

This software on the Arch Linux repository does not support the pcre2 engine.

I still need backreference, but this package on the official Arch Linux repository does not support the pcre2 feature, I hope to add.

Thank you very much!

---

_Comment by @BurntSushi on 2018-09-06 14:33_

ripgrep with PCRE2 support hasn't been released yet, so the Arch package can't support it.

Additionally, I don't maintain the ripgrep package in the Archlinux repository, so this is not the place to file packaging bugs. Instead, you should file bugs with downstream: https://www.archlinux.org/packages/community/x86_64/ripgrep/ (But again, to be clear, this isn't a bug because ripgrep hasn't been released with PCRE2 support yet.)

Until the next release, if you want PCRE2 support, you'll need to build ripgrep yourself. It should only take a couple minutes at most on Archlinux: https://github.com/BurntSushi/ripgrep#building

---

_Closed by @BurntSushi on 2018-09-06 14:33_

---

_Comment by @yuzumx on 2018-09-06 22:33_

Thank you！

---
