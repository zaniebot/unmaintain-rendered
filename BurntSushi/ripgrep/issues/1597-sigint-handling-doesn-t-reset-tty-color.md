```yaml
number: 1597
title: "SIGINT handling doesn't reset tty color"
type: issue
state: closed
author: rorytm
labels:
  - duplicate
assignees: []
created_at: 2020-05-26T21:23:37Z
updated_at: 2020-05-26T21:33:54Z
url: https://github.com/BurntSushi/ripgrep/issues/1597
synced_at: 2026-01-12T16:13:23Z
```

# SIGINT handling doesn't reset tty color

---

_@rorytm_

#### What version of ripgrep are you using?

ripgrep 12.1.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

pacman

#### What operating system are you using ripgrep on?

Linux / Arch / 5.6.14-1-ck-skylake

#### Describe your bug.
When the process closes via sigint (ctrl + c), ripgrep doesn't revert the tty color before exiting.

#### What are the steps to reproduce the behavior?

Interrupt the process while it's printing a colored character.
```
rg . /
```
`ctrl + c`

#### What is the actual behavior?
tty continues printing in the most recent color that ripgrep was using


#### What is the expected behavior?
tty should go back to the primary color

What do you think ripgrep should have done?
ripgrep should go back to the primary color before exiting after receiving sigint

---

_Comment by @BurntSushi on 2020-05-26 21:33_

https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#stop-ripgrep

---

_Closed by @BurntSushi on 2020-05-26 21:33_

---

_Label `duplicate` added by @BurntSushi on 2020-05-26 21:33_

---
