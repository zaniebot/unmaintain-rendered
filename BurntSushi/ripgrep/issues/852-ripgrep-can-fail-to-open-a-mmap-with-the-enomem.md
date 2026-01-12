```yaml
number: 852
title: ripgrep can fail to open a mmap with the ENOMEM error
type: issue
state: closed
author: BurntSushi
labels:
  - bug
assignees: []
created_at: 2018-03-10T12:51:20Z
updated_at: 2018-03-10T13:13:48Z
url: https://github.com/BurntSushi/ripgrep/issues/852
synced_at: 2026-01-12T16:13:22Z
```

# ripgrep can fail to open a mmap with the ENOMEM error

---

_@BurntSushi_

#### What version of ripgrep are you using?

0.8.1

#### What operating system are you using ripgrep on?

Linux

#### Describe your question, feature request, or bug.

It is possible for ripgrep to fail to open a memory map with the `ENOMEM` error. In particular, I believe this can happen if one tries to search a file that is bigger than available memory. I had thought this case would be OK since the OS could handle swapping the file into and out of memory, but I now wonder whether this is caused by overcommit settings?

No matter the cause, if the mmap fails with ENOMEM, then we should probably just fall back to standard read/write calls. (One wonders whether we should do this regardless of how mmap fails.)

This bug was found as part of the investigation in #850.

---

_Label `bug` added by @BurntSushi on 2018-03-10 12:51_

---

_Closed by @BurntSushi on 2018-03-10 13:13_

---
