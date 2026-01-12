```yaml
number: 6886
title: "Adding support for `.pyc`  files in `uv run`"
type: pull_request
state: merged
author: vss96
labels:
  - enhancement
assignees: []
merged: true
base: main
head: pyc-ext
created_at: 2024-08-30T19:48:43Z
updated_at: 2024-08-30T23:45:24Z
url: https://github.com/astral-sh/uv/pull/6886
synced_at: 2026-01-12T16:07:34Z
```

# Adding support for `.pyc`  files in `uv run`

---

_@vss96_

## Summary
- The change relates to #6635 is to include compiled python files (.pyc) in the uv run command.
- After this change `uv run foo.pyc` should spawn `python foo.pyc`.


## Test Plan
- There is a test that uses TestContext to compile and run a simple python file that prints "Hello World".
- I built the project locally and tried the same with a simple python file that I had compiled.




---

_@charliermarsh approved on 2024-08-30 23:33_

Thanks!

---

_Renamed from "Adding support for .pyc files" to "Adding support for `.pyc`  files in `uv run`" by @charliermarsh on 2024-08-30 23:38_

---

_Label `enhancement` added by @charliermarsh on 2024-08-30 23:38_

---

_Merged by @charliermarsh on 2024-08-30 23:45_

---

_Closed by @charliermarsh on 2024-08-30 23:45_

---
