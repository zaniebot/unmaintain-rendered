```yaml
number: 1433
title: "Read from `/bin/sh` if `/bin/ls` cannot be found when determing libc path"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/read-sh
created_at: 2024-02-16T05:14:15Z
updated_at: 2024-02-20T10:57:28Z
url: https://github.com/astral-sh/uv/pull/1433
synced_at: 2026-01-12T16:04:37Z
```

# Read from `/bin/sh` if `/bin/ls` cannot be found when determing libc path

---

_@zanieb_

I'm not sure if we should just switch to _always_ reading from sh instead? I don't love that all these errors are strings and I if `/bin/ls` exists but can't be parsed we still won't try `/bin/sh`. We may want to address these things in the future.

Closes https://github.com/astral-sh/uv/issues/1395

---

_Label `bug` added by @zanieb on 2024-02-16 05:14_

---

_Marked ready for review by @zanieb on 2024-02-16 05:21_

---

_@BurntSushi approved on 2024-02-16 12:51_

I think we should bring this in as-is since it fixes a known problem, but I do agree that probably just checking `/bin/sh` is sufficient.

---

_Merged by @BurntSushi on 2024-02-16 12:51_

---

_Closed by @BurntSushi on 2024-02-16 12:51_

---

_Branch deleted on 2024-02-16 12:51_

---
