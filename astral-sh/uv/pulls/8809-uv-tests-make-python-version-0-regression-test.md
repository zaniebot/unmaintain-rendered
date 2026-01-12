```yaml
number: 8809
title: "uv/tests: make `python_version < '0'` regression test smaller"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/faster-version-zero-test
created_at: 2024-11-04T15:14:27Z
updated_at: 2024-11-04T16:10:20Z
url: https://github.com/astral-sh/uv/pull/8809
synced_at: 2026-01-12T16:08:30Z
```

# uv/tests: make `python_version < '0'` regression test smaller

---

_@BurntSushi_

Running this test manually on the latest release shows that it still
emits a `python_version < '0'` marker:

    $ uv-0.4.29 pip compile requirements.in -c constraints.txt --annotation-style line --python 3.10 --universal | rg "< '0'"
    Resolved 151 packages in 97ms
    apache-airflow-providers-common-sql==1.19.0 ; python_version < '0'  # via apache-airflow-providers-sqlite

That is, even though this is a smaller test, it's still testing the same
bug.

This is about twice as fast on my machine. It's probably still worth
moving to a packse test, but this was quick to do.


---

_Review requested from @konstin by @BurntSushi on 2024-11-04 15:14_

---

_Merged by @BurntSushi on 2024-11-04 16:10_

---

_Closed by @BurntSushi on 2024-11-04 16:10_

---

_Branch deleted on 2024-11-04 16:10_

---
