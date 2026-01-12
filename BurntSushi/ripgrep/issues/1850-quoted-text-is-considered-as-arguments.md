```yaml
number: 1850
title: Quoted text is considered as arguments
type: issue
state: closed
author: mmahmoudian
labels: []
assignees: []
created_at: 2021-04-16T09:35:31Z
updated_at: 2021-04-16T11:14:43Z
url: https://github.com/BurntSushi/ripgrep/issues/1850
synced_at: 2026-01-12T16:13:24Z
```

# Quoted text is considered as arguments

---

_@mmahmoudian_

#### What version of ripgrep are you using?

ripgrep 12.1.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Pacman official "community" repo

#### What operating system are you using ripgrep on?

```
❯  inxi --system
System:
  Host: precisiontower Kernel: 5.9.16-1-MANJARO x86_64 bits: 64
  Desktop: KDE Plasma 5.21.3 Distro: Manjaro Linux
```

#### Describe your bug.

Quoted text is still considered as arguments. For example `rg "--help"` will print out the help of ripgrep rather than searching for the string in the current path.

#### What are the steps to reproduce the behavior?

Compare  `rg --help` and `rg "--help"`

#### What is the actual behavior?

The output of `rg --help` and `rg "--help"` are identical, but the  `rg "--help"` should be treated as `rg -e "--help"`.

#### What is the expected behavior?

The quoted text should be considered as pattern to be searched


---

_Comment by @BurntSushi on 2021-04-16 11:14_

This is how shells work. ripgrep doesn't see the quotes. The work around is documented in the man page: https://github.com/BurntSushi/ripgrep/blob/92286ad4d2a9c8b054de8aaea05c23a7220d20b3/doc/rg.1.txt.tpl#L73

---

_Closed by @BurntSushi on 2021-04-16 11:14_

---
