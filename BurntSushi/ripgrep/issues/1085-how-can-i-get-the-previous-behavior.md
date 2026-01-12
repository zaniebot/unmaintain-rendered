```yaml
number: 1085
title: How can I get the previous behavior?
type: issue
state: closed
author: Yggdroot
labels:
  - question
assignees: []
created_at: 2018-10-17T07:23:26Z
updated_at: 2018-10-17T11:11:15Z
url: https://github.com/BurntSushi/ripgrep/issues/1085
synced_at: 2026-01-12T16:13:22Z
```

# How can I get the previous behavior?

---

_@Yggdroot_

I update the `rg` recently, and find the behavior of `--files` has been changed.

Before:  ripgrep 0.5.2

output of `rg --files .` is :
```
aaa/test.txt
bbb/test2.txt
```
Now: ripgrep 0.10.0
output of `rg --files .` is :
```
./aaa/test.txt
./bbb/test2.txt
```
How can I get the previous behavior?

---

_Comment by @BurntSushi on 2018-10-17 11:11_

By omitting the `.`.

---

_Closed by @BurntSushi on 2018-10-17 11:11_

---

_Label `question` added by @BurntSushi on 2018-10-17 11:11_

---
