```yaml
number: 1030
title: rg silently exits when greping certain patterns using -A/-B
type: issue
state: closed
author: hongxuchen
labels:
  - bug
assignees: []
created_at: 2018-08-28T13:06:11Z
updated_at: 2018-08-28T13:31:07Z
url: https://github.com/BurntSushi/ripgrep/issues/1030
synced_at: 2026-01-12T16:13:22Z
```

# rg silently exits when greping certain patterns using -A/-B

---

_@hongxuchen_

#### What version of ripgrep are you using?
ripgrep 0.9.0
-SIMD -AVX

#### How did you install ripgrep?
cargo install (nightly)

#### What operating system are you using ripgrep on?
x86_64 GNU/Linux Ubuntu 18.04

#### Describe your question, feature request, or bug.

When using options like `-A` or `-B`, rg may exit silently when matching some patterns.


#### If this is a bug, what are the steps to reproduce the behavior?
[hang-res.zip](https://github.com/BurntSushi/ripgrep/files/2328104/hang-res.zip)


![2018-08-28-205721_562x498_scrot](https://user-images.githubusercontent.com/843267/44724467-5986e900-ab05-11e8-8cd2-19192649083e.png)

#### If this is a bug, what is the actual behavior?
`-A` version should print matched lines and lines after.

What do you think ripgrep should have done?
N.A.


---

_Comment by @BurntSushi on 2018-08-28 13:13_

Interesting! I don't know what's wrong here, but this bug is fixed on master, which includes a rewrite of the relevant code. I was able to reproduce this on the current release of ripgrep (0.9.0). Thanks for the great bug report!

---

_Closed by @BurntSushi on 2018-08-28 13:13_

---

_Label `bug` added by @BurntSushi on 2018-08-28 13:13_

---

_Comment by @hongxuchen on 2018-08-28 13:31_

Confirmed that the issue no longer exists as of 8f978a3 :smile: 

---
