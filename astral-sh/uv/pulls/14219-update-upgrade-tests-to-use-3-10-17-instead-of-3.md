```yaml
number: 14219
title: Update upgrade tests to use 3.10.17 instead of 3.10.8
type: pull_request
state: merged
author: jtfmumm
labels:
  - internal
assignees: []
merged: true
base: main
head: jtfm/update-upgrade-tests
created_at: 2025-06-23T16:00:22Z
updated_at: 2025-06-23T16:19:38Z
url: https://github.com/astral-sh/uv/pull/14219
synced_at: 2026-01-12T16:11:05Z
```

# Update upgrade tests to use 3.10.17 instead of 3.10.8

---

_@jtfmumm_

@oconnor663 discovered that executing `3.10.8` on Arch Linux ran into an error loading `libcrypt.so.1`. This caused uv to install the latest patch version on `uv venv` operations during upgrade tests, which undermined their purpose (since they are checking that if you first install `3.10.8` and then upgrade, virtual environments are transparently upgraded). This PR updates the test to use `3.10.17` instead to avoid this issue.


---

_Label `internal` added by @jtfmumm on 2025-06-23 16:00_

---

_Review requested from @oconnor663 by @jtfmumm on 2025-06-23 16:02_

---

_Comment by @oconnor663 on 2025-06-23 16:07_

Confirmed that on the machine where I can't execute 3.10.8:

```
$ uvx python@3.10.8
error: Querying Python at `/home/jacko/.local/share/uv/python/cpython-3.10.8-linux-x86_64-gnu/bin/python3.10` failed with exit status exit status: 127

[stderr]
/home/jacko/.local/share/uv/python/cpython-3.10.8-linux-x86_64-gnu/bin/python3.10: error while loading shared libraries: libcrypt.so.1: cannot open shared object file: No such file or directory
```

All the tests still pass on this branch:

```
$ cargo nextest run
...
Summary [  76.847s] 2709 tests run: 2709 passed (28 slow), 4 skipped
```


---

_@oconnor663 approved on 2025-06-23 16:08_

---

_Marked ready for review by @jtfmumm on 2025-06-23 16:19_

---

_Merged by @jtfmumm on 2025-06-23 16:19_

---

_Closed by @jtfmumm on 2025-06-23 16:19_

---

_Branch deleted on 2025-06-23 16:19_

---
