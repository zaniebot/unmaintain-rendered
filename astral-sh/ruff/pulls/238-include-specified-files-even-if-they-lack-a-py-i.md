```yaml
number: 238
title: "Include specified files, even if they lack a .py[i] extension"
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: extension
created_at: 2022-09-21T00:49:08Z
updated_at: 2022-09-21T00:53:58Z
url: https://github.com/astral-sh/ruff/pull/238
synced_at: 2026-01-12T05:48:45Z
```

# Include specified files, even if they lack a .py[i] extension

---

_Pull request opened by @andersk on 2022-09-21 00:49_

Some Python scripts don’t have a `.py` extension (especially executables with `#!` lines).  We should only filter the extension of files found within a specified directory (`ruff .`), not of specified files (`ruff my-script`).

---

_Merged by @charliermarsh on 2022-09-21 00:53_

---

_Closed by @charliermarsh on 2022-09-21 00:53_

---

_Branch deleted on 2022-09-21 00:53_

---
