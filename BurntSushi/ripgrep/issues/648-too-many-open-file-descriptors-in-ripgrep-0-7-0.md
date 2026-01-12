```yaml
number: 648
title: too many open file descriptors in ripgrep 0.7.0
type: issue
state: closed
author: BurntSushi
labels:
  - bug
assignees: []
created_at: 2017-10-22T14:29:40Z
updated_at: 2017-10-22T14:33:14Z
url: https://github.com/BurntSushi/ripgrep/issues/648
synced_at: 2026-01-12T16:13:22Z
```

# too many open file descriptors in ripgrep 0.7.0

---

_@BurntSushi_

There is a very bad bug in the recently released ripgrep 0.7.0 where a [misguided optimization](https://github.com/BurntSushi/ripgrep/issues/633#issuecomment-338472637) results in too many open file descriptors.

I'm in the process of putting out a ripgrep 0.7.1

---

_Label `bug` added by @BurntSushi on 2017-10-22 14:29_

---

_Assigned to @BurntSushi by @BurntSushi on 2017-10-22 14:29_

---

_Closed by @BurntSushi on 2017-10-22 14:33_

---
