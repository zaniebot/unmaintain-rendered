```yaml
number: 4523
title: "uv/tests: tweak toolchain_find test"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
  - testing
assignees: []
merged: true
base: main
head: ag/fix-toolchain-find-test
created_at: 2024-06-25T17:02:02Z
updated_at: 2024-06-25T17:23:37Z
url: https://github.com/astral-sh/uv/pull/4523
synced_at: 2026-01-10T13:48:28Z
```

# uv/tests: tweak toolchain_find test

---

_Pull request opened by @BurntSushi on 2024-06-25 17:02_

I was getting this test failure locally on my Archlinux system:

```
-old snapshot
+new results
    0     0 │ success: true
    1     1 │ exit_code: 0
    2     2 │ ----- stdout -----
    3       │-[PYTHON-3.12]
          3 │+/usr/bin/python3
    4     4 │
    5     5 │ ----- stderr -----
```

Where I have `/usr/bin/python3` and `/usr/bin/python3.12`.

Thanks @zanieb for the help with figuring out the fix here!


---

_Review requested from @zanieb by @BurntSushi on 2024-06-25 17:02_

---

_@zanieb reviewed on 2024-06-25 17:04_

---

_Review comment by @zanieb on `crates/uv/tests/toolchain_find.rs`:109 on 2024-06-25 17:04_

```suggestion
```

---

_@zanieb approved on 2024-06-25 17:04_

---

_Label `internal` added by @zanieb on 2024-06-25 17:06_

---

_Label `testing` added by @zanieb on 2024-06-25 17:06_

---

_Merged by @BurntSushi on 2024-06-25 17:23_

---

_Closed by @BurntSushi on 2024-06-25 17:23_

---

_Branch deleted on 2024-06-25 17:23_

---
