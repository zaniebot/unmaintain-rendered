```yaml
number: 3064
title: "Allow passing a venv to `uv pip --python`"
type: pull_request
state: merged
author: pfmoore
labels:
  - enhancement
assignees: []
merged: true
base: main
head: python_venv
created_at: 2024-04-16T15:27:49Z
updated_at: 2024-04-16T18:54:44Z
url: https://github.com/astral-sh/uv/pull/3064
synced_at: 2026-01-10T14:43:31Z
```

# Allow passing a venv to `uv pip --python`

---

_Pull request opened by @pfmoore on 2024-04-16 15:27_

Fixes https://github.com/astral-sh/uv/issues/3060

## Summary

Allows passing a virtual environment (the path to the directory, rather than the path to the Python interpreter within the directory) to the `--python` option of the `uv pip` command.

## Test Plan

Tested manually to confirm that the expected new functionality works. The test suite still passes after this change.

I don't know how to add tests for a new feature like this. I would be happy to do so if someone can give me some pointers on how to do it.


---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-16 16:09_

---

_Label `enhancement` added by @charliermarsh on 2024-04-16 17:07_

---

_@charliermarsh approved on 2024-04-16 18:13_

Thanks! I tweaked the control flow to use a single metadata lookup.

---

_Comment by @charliermarsh on 2024-04-16 18:14_

I think this also closes #3062?

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/find_python.rs`:66 on 2024-04-16 18:16_

We could consider erroring here if the request contains a separator, though I suppose it's not strictly necessary? The error in that case looks like `error: Failed to locate Python interpreter at `.venv/foo/python`` already.

---

_@charliermarsh reviewed on 2024-04-16 18:16_

---

_Renamed from "Allow passing a venv to uv pip --python" to "Allow passing a venv to `uv pip --python`" by @charliermarsh on 2024-04-16 18:16_

---

_Comment by @charliermarsh on 2024-04-16 18:24_

Will fix the failure, need https://github.com/astral-sh/uv/pull/3072 first.

---

_Merged by @charliermarsh on 2024-04-16 18:39_

---

_Closed by @charliermarsh on 2024-04-16 18:39_

---

_@pfmoore reviewed on 2024-04-16 18:41_

---

_Review comment by @pfmoore on `crates/uv-interpreter/src/find_python.rs`:58 on 2024-04-16 18:41_

I don't personally care, but this probably won't handle environments like cygwin or mingw64, as I think they use a `bin` directory, because they are emulating Unix. They will presumably still have a `.exe` suffix, though, as that's a Windows requirement.

I suspect it's better to ignore this problem until someone using one of those environments speaks up, because they will be able to give better information than my guesses...

---

_@pfmoore reviewed on 2024-04-16 18:42_

---

_Review comment by @pfmoore on `crates/uv-interpreter/src/find_python.rs`:66 on 2024-04-16 18:42_

Yeah, I don't think it's worth a special check.

---

_@pfmoore reviewed on 2024-04-16 18:44_

Looks like you merged while I was commenting!

---

_Branch deleted on 2024-04-16 18:45_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/find_python.rs`:58 on 2024-04-16 18:52_

Yeah makes total sense. I think we _already_ have this problem in a few places so I'm okay with using the same pattern until we fix it holistically...

---

_@charliermarsh reviewed on 2024-04-16 18:52_

---

_Comment by @charliermarsh on 2024-04-16 18:52_

Oh sorry, I set to auto-merge.

---

_Comment by @pfmoore on 2024-04-16 18:54_

Not a problem. And thanks for (improving and) merging this - glad my beginner-level Rust was worth it!

---
