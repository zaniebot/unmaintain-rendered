```yaml
number: 4214
title: Allow version specifiers to be used in Python version requests
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/version-request-range
created_at: 2024-06-10T19:51:28Z
updated_at: 2024-06-10T23:20:10Z
url: https://github.com/astral-sh/uv/pull/4214
synced_at: 2026-01-12T16:06:05Z
```

# Allow version specifiers to be used in Python version requests

---

_@zanieb_

In service of https://github.com/astral-sh/uv/issues/4212 but this is user-facing e.g. Python discovery will support version specifiers everywhere now.

Closes https://github.com/astral-sh/uv/issues/4212 

---

_Marked ready for review by @zanieb on 2024-06-10 20:12_

---

_Comment by @zanieb on 2024-06-10 20:13_

A little terrifying...

```
❯ cargo run -q -- pip install --python '>=3.8' anyio -v
DEBUG Searching for Python >=3.8 in all sources
DEBUG Found CPython 3.12.3 at `/Users/zb/workspace/uv/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.12.3 environment at .venv/bin/python3
DEBUG Acquired lock for `.venv`
DEBUG Requirement satisfied: anyio
DEBUG Requirement satisfied: idna>=2.8
DEBUG Requirement satisfied: sniffio>=1.1
Audited 1 package in 15ms
```

---

_@zanieb reviewed on 2024-06-10 20:36_

---

_Review comment by @zanieb on `crates/uv/tests/lock.rs`:2082 on 2024-06-10 20:36_

This is problematic, need to add a filter for this

---

_@zanieb reviewed on 2024-06-10 20:36_

---

_Review comment by @zanieb on `crates/uv/tests/lock.rs`:2089 on 2024-06-10 20:36_

And this should be filtered too, right?

---

_@zanieb reviewed on 2024-06-10 20:37_

---

_Review comment by @zanieb on `crates/uv/tests/lock.rs`:2082 on 2024-06-10 20:37_

https://github.com/astral-sh/uv/blob/90947a933df2fe3f42bc9cbbf27dbce3a3b3a708/crates/uv/src/commands/project/mod.rs#L151-L156

---

_Review comment by @zanieb on `crates/uv/tests/lock.rs`:2089 on 2024-06-10 20:40_

I guess this is because it's from an old test context that we used to create the lockfile...

---

_@zanieb reviewed on 2024-06-10 20:40_

---

_@charliermarsh reviewed on 2024-06-10 20:46_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:2089 on 2024-06-10 20:46_

Yes you need to use the old context for the filter. We do this in a different test.

---

_@charliermarsh reviewed on 2024-06-10 20:48_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:2089 on 2024-06-10 20:48_

I can't find it now, maybe that other test got changed. But yes need old filter here.

---

_@charliermarsh approved on 2024-06-10 20:51_

---

_Comment by @zanieb on 2024-06-10 21:54_

Waiting for the fixes at

- #4218 
- #4222 

---

_Merged by @zanieb on 2024-06-10 23:20_

---

_Closed by @zanieb on 2024-06-10 23:20_

---

_Branch deleted on 2024-06-10 23:20_

---
