```yaml
number: 3623
title: Add basic tests for lockfile generation
type: pull_request
state: merged
author: charliermarsh
labels:
  - testing
assignees: []
merged: true
base: main
head: charlie/git-lock
created_at: 2024-05-15T20:22:12Z
updated_at: 2024-05-16T15:18:10Z
url: https://github.com/astral-sh/uv/pull/3623
synced_at: 2026-01-12T16:05:45Z
```

# Add basic tests for lockfile generation

---

_@charliermarsh_

_No description provided._

---

_Comment by @charliermarsh on 2024-05-15 20:22_

@BurntSushi - do you find this useful or intrusive?

---

_Label `testing` added by @charliermarsh on 2024-05-15 20:22_

---

_Marked ready for review by @charliermarsh on 2024-05-15 20:22_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-05-15 20:22_

---

_Comment by @charliermarsh on 2024-05-16 01:19_

(Alternatively, I can write these as `uv lock` and `uv sync` invocations if we want, it just requires more work to get all the options like `--exclude-newer` threaded down.)

---

_@BurntSushi approved on 2024-05-16 12:50_

I think it's useful! We'll certainly need something like this.

I think my main concern here is the encoding of the `--unstable-uv-lock-file` flag in tests. Because we want to remove that ~soonish. But I think these tests are probably simple/straight-forward enough that we ought to be able to migrate them to Blue Jay without much fuss.

---

_Comment by @charliermarsh on 2024-05-16 15:18_

Sounds good -- I can also mechanically migrate them to `uv lock` and `uv sync`, it just requires adding support for (e.g.) `--exclude-newer`.

---

_Merged by @charliermarsh on 2024-05-16 15:18_

---

_Closed by @charliermarsh on 2024-05-16 15:18_

---

_Branch deleted on 2024-05-16 15:18_

---
