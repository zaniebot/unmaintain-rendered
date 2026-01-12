```yaml
number: 217
title: "Detect multi-target assignment as unpacking if *any* target is unpacking"
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: multi-unpack
created_at: 2022-09-17T01:45:52Z
updated_at: 2022-09-17T03:45:44Z
url: https://github.com/astral-sh/ruff/pull/217
synced_at: 2026-01-12T15:55:04Z
```

# Detect multi-target assignment as unpacking if *any* target is unpacking

---

_@andersk_

Improves the fix for #161.

---

_Comment by @charliermarsh on 2022-09-17 03:43_

Looks good to me. I think Flake8 detects `baz` as unused if you remove the `print` (but ruff does not), but this is an improvement.

---

_Merged by @charliermarsh on 2022-09-17 03:45_

---

_Closed by @charliermarsh on 2022-09-17 03:45_

---
