```yaml
number: 11218
title: "Allow discovering virtual environments from the first interpreter found on the `PATH`"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/python-find-first
created_at: 2025-02-04T16:15:25Z
updated_at: 2025-02-04T21:41:39Z
url: https://github.com/astral-sh/uv/pull/11218
synced_at: 2026-01-12T16:09:44Z
```

# Allow discovering virtual environments from the first interpreter found on the `PATH`

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/11214

Special-cases the first Python executable we find on the `PATH`, allowing it to be considered during searches for virtual environments.

For some context, there are two stages to Python interpreter discovery

1. We find possible Python executables in various sources
2. We query the executables to determine canonical metadata about the interpreter

We can't really be "sure" if an executable is a complaint virtual environment during (1), we need to query the interpreter first. This means that if you're only allowed to installed into virtual environments, we'll query every interpreter on your PATH. This is not performant, and causes confusion for users. Notably, I recently improved error messaging when we can't find any valid interpreters, by showing the error message we encounter while querying an interpreter (if any). However, this is problematic when there's an error for an interpreter that is not relevant to your search. In https://github.com/astral-sh/uv/pull/11143, I added filtering to avoid querying additional interpreters, but that regressed some user experiences where they were relying on us finding implicitly active virtual environments via the PATH.

---

_Label `bug` added by @zanieb on 2025-02-04 16:15_

---

_@zanieb reviewed on 2025-02-04 19:52_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:7611 on 2025-02-04 19:52_

This test case regresses, undoing the work from https://github.com/astral-sh/uv/pull/11143 if the interpreter is at the front of the PATH. We don't see the regression in the diff because I added a new test case below which retains the previous error message, i.e., we do the right thing if the broken interpreter is not at the front of the PATH. We can do better, but the regression this pull requests fixes seems more important to address?

---

_Comment by @zanieb on 2025-02-04 19:57_

Looking back at https://github.com/astral-sh/uv/issues/2109 and https://github.com/astral-sh/uv/issues/4009, it seems likely that #11143 regressed behavior there. This _sort_ of fixes that? It's basically a reimplementation of https://github.com/astral-sh/uv/pull/4032

---

_Comment by @zanieb on 2025-02-04 20:18_

An alternative approach would be something like 1467172726d9dcf4d283bc92ce351fd6cee8d7c6 which.. could also have problems, i.e., exclude interpreters incorrectly and requires querying more directories on the `PATH` when looking for virtual environments.

---

_Marked ready for review by @zanieb on 2025-02-04 20:48_

---

_@charliermarsh approved on 2025-02-04 21:11_

---

_Merged by @zanieb on 2025-02-04 21:41_

---

_Closed by @zanieb on 2025-02-04 21:41_

---

_Branch deleted on 2025-02-04 21:41_

---
