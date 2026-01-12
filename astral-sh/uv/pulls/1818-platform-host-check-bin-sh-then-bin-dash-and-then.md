```yaml
number: 1818
title: "platform-host: check /bin/sh, then /bin/dash and then /bin/ls"
type: pull_request
state: merged
author: BurntSushi
labels:
  - bug
assignees: []
merged: true
base: main
head: ag/fix-i1810
created_at: 2024-02-21T16:02:52Z
updated_at: 2024-02-21T16:49:56Z
url: https://github.com/astral-sh/uv/pull/1818
synced_at: 2026-01-12T16:04:45Z
```

# platform-host: check /bin/sh, then /bin/dash and then /bin/ls

---

_@BurntSushi_

Previously, we were only checking /bin/sh. While that works in most
cases, it seems like there are still scenarios where /bin/sh isn't an
executable itself, and is instead just a shell script that calls
/bin/dash. (See #1810 for example.)

In this PR, we make the `ld` detection a bit more robust by trying
multiple paths. As with previous changes, we emit copious logs to help
debug this in the future.

It's not totally clear how to test this. I'm not sure how to reproduce
the environment mentions in #1810 specifically since it seems like an
internal variant of WSL Ubuntu.

Fixes #1810


---

_Review requested from @konstin by @BurntSushi on 2024-02-21 16:03_

---

_Review requested from @zanieb by @BurntSushi on 2024-02-21 16:03_

---

_@zanieb approved on 2024-02-21 16:47_

---

_Merged by @BurntSushi on 2024-02-21 16:49_

---

_Closed by @BurntSushi on 2024-02-21 16:49_

---

_Branch deleted on 2024-02-21 16:49_

---

_Label `bug` added by @BurntSushi on 2024-02-21 16:49_

---
