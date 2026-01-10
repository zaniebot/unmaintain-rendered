```yaml
number: 1046
title: Canonicalize paths to interpreter executables before checking modified time
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/canon
created_at: 2024-01-22T21:16:55Z
updated_at: 2024-01-22T21:44:23Z
url: https://github.com/astral-sh/uv/pull/1046
synced_at: 2026-01-10T15:39:03Z
```

# Canonicalize paths to interpreter executables before checking modified time

---

_Pull request opened by @zanieb on 2024-01-22 21:16_

If the executable is a symbolic link, checking the modified time will not reflect changes to the source file e.g.

```
❯ touch foo
❯ ln -s foo foobar
❯ gstat -c %Y foo
1705958431
❯ gstat -c %Y foobar
1705958438
❯ touch foo
❯ gstat -c %Y foobar
1705958438
```

This can result in a stale cache being treated as fresh; for example, when Rye changes the interpreter linked in a virtual environment.

---

_@charliermarsh approved on 2024-01-22 21:21_

---

_Review comment by @charliermarsh on `crates/puffin-interpreter/src/interpreter.rs`:284 on 2024-01-22 21:21_

Is there `fs_err::canonicalize`?

---

_@charliermarsh reviewed on 2024-01-22 21:21_

---

_Marked ready for review by @zanieb on 2024-01-22 21:23_

---

_Label `bug` added by @zanieb on 2024-01-22 21:31_

---

_Merged by @zanieb on 2024-01-22 21:44_

---

_Closed by @zanieb on 2024-01-22 21:44_

---

_Branch deleted on 2024-01-22 21:44_

---
