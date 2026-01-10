```yaml
number: 1728
title: Expose find_uv_bin and declare typing support
type: pull_request
state: merged
author: gaborbernat
labels:
  - enhancement
assignees: []
merged: true
base: main
head: expose-find-uv-bin-with-types
created_at: 2024-02-20T01:59:43Z
updated_at: 2024-02-20T16:37:51Z
url: https://github.com/astral-sh/uv/pull/1728
synced_at: 2026-01-10T15:33:24Z
```

# Expose find_uv_bin and declare typing support

---

_Pull request opened by @gaborbernat on 2024-02-20 01:59_

Resolves https://github.com/astral-sh/uv/issues/1677

---

_@gaborbernat reviewed on 2024-02-20 02:00_

---

_Review comment by @gaborbernat on `python/uv/__main__.py`:7 on 2024-02-20 02:00_

Marked this as private explicitly 👍 because by default in Python anything that's no prohibited (at least by convention in case of `_`) is allowed.

---

_@gaborbernat reviewed on 2024-02-20 02:01_

---

_Review comment by @gaborbernat on `python/uv/__main__.py`:7 on 2024-02-20 02:01_

Do we have tests for these files? I don't see any Python test harness just yet 🤔 

---

_Comment by @zanieb on 2024-02-20 02:01_

Thanks for contributing!

This is a duplicate of https://github.com/astral-sh/uv/pull/1685 but there were some problems there and this looks correct to me.

---

_@gaborbernat reviewed on 2024-02-20 02:02_

---

_@zanieb reviewed on 2024-02-20 02:02_

---

_Review comment by @gaborbernat on `python/uv/__main__.py`:25 on 2024-02-20 02:02_

By moving such logic into a private `_run` method, we avoid exposing all global variables as publicly import-able by anyone.

---

_Review comment by @zanieb on `python/uv/__main__.py`:7 on 2024-02-20 02:02_

Nope. We should probably `pip install uv` and `python -m uv` smoke test in CI when we build wheels for releases? We could track this separately.

---

_@zanieb reviewed on 2024-02-20 02:03_

---

_Review comment by @zanieb on `python/uv/__main__.py`:7 on 2024-02-20 02:03_

We could also probably install directly e.g. `uv pip install -e ./python/uv` and then test in the "Smoke test" step?

---

_Label `enhancement` added by @zanieb on 2024-02-20 02:04_

---

_@gaborbernat reviewed on 2024-02-20 02:05_

---

_Review comment by @gaborbernat on `python/uv/__main__.py`:7 on 2024-02-20 02:05_

Yeah, agreed, though likely should be a separate PR 😊 Up to you if you want to wait for this until that lands.

---

_@zanieb reviewed on 2024-02-20 15:21_

---

_Review comment by @zanieb on `python/uv/__main__.py`:7 on 2024-02-20 15:21_

Feel free to put that up if you're interested! :) unless we get to it first

---

_Merged by @zanieb on 2024-02-20 15:21_

---

_Closed by @zanieb on 2024-02-20 15:21_

---

_Branch deleted on 2024-02-20 16:37_

---
