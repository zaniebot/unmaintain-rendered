```yaml
number: 4443
title: Fix construction of Python path in test context
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - testing
assignees: []
merged: true
base: main
head: zb/toolchain-fix-iii
created_at: 2024-06-21T21:39:01Z
updated_at: 2024-06-23T15:05:48Z
url: https://github.com/astral-sh/uv/pull/4443
synced_at: 2026-01-10T13:48:28Z
```

# Fix construction of Python path in test context

---

_Pull request opened by @zanieb on 2024-06-21 21:39_

When executables were not named `python3` e.g. `python3.11` we would construct a Python path that would only work for _some_ requests in tests since we don't search for those names unless a specific version is requested. To solve, we construct a test context with constant Python executable names. For example, if a test context was created with `3.11` and `3.12` we could end up with the search path `/usr/local/python-3.11/bin:/usr/local/python-3.12/bin` where the executables are named `python3.11` and `python3` respectively. A test invocation of uv requesting any Python toolchain version would then locate the `3.12` executable since the `3.11` executable doesn't have the generic name, but we want `3.11` to come first.

On Windows, we just leave things as-is because executables are always called `python.exe`.

Closes https://github.com/astral-sh/uv/issues/4376

---

_Label `internal` added by @zanieb on 2024-06-21 21:39_

---

_Label `testing` added by @zanieb on 2024-06-21 21:39_

---

_Comment by @mgorny on 2024-06-22 10:09_

Thanks a lot! In fact, this seems to fix all the (subsequent) test failures I was seeing recently.

---

_Comment by @zanieb on 2024-06-22 14:29_

Thanks for giving it a try! This was definitely a fundamental problem — I encountered it on my machine too.

---

_@charliermarsh approved on 2024-06-23 15:00_

---

_Merged by @zanieb on 2024-06-23 15:05_

---

_Closed by @zanieb on 2024-06-23 15:05_

---

_Branch deleted on 2024-06-23 15:05_

---
