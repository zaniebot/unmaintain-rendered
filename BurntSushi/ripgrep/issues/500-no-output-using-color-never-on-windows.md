```yaml
number: 500
title: "no output using `--color never` on Windows "
type: issue
state: closed
author: Yggdroot
labels:
  - duplicate
assignees: []
created_at: 2017-06-02T08:31:15Z
updated_at: 2017-10-22T01:40:34Z
url: https://github.com/BurntSushi/ripgrep/issues/500
synced_at: 2026-01-12T16:13:22Z
```

# no output using `--color never` on Windows 

---

_@Yggdroot_

`rg --version`: ripgrep 0.5.2
Windows 10, x64
There is output using `rg . --files`, but no output using `rg . --files --color never`.

---

_Comment by @elirnm on 2017-06-03 21:10_

Probably related to https://github.com/BurntSushi/ripgrep/issues/466

---

_Comment by @BurntSushi on 2017-10-22 01:40_

My guess is that this is a duplicate of #466 (which I can't reproduce).

---

_Closed by @BurntSushi on 2017-10-22 01:40_

---

_Label `duplicate` added by @BurntSushi on 2017-10-22 01:40_

---
