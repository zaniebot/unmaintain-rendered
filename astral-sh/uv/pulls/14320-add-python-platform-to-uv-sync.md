```yaml
number: 14320
title: "Add `--python-platform` to `uv sync`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/plat
created_at: 2025-06-27T17:48:37Z
updated_at: 2025-07-11T01:38:29Z
url: https://github.com/astral-sh/uv/pull/14320
synced_at: 2026-01-12T16:11:09Z
```

# Add `--python-platform` to `uv sync`

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/14273.


---

_Label `enhancement` added by @charliermarsh on 2025-06-27 17:48_

---

_Marked ready for review by @charliermarsh on 2025-06-27 17:48_

---

_@jtfmumm approved on 2025-07-10 12:58_

Generally looks good, but I agree `--python-version` is confusing given that `--python` also accepts a Python version. Maybe `--python-minimum` or something along those lines?

---

_Renamed from "Add `--python-platform` and `--python-version` to `uv sync`" to "Add `--python-platform` to `uv sync`" by @charliermarsh on 2025-07-11 01:29_

---

_Merged by @charliermarsh on 2025-07-11 01:38_

---

_Closed by @charliermarsh on 2025-07-11 01:38_

---

_Branch deleted on 2025-07-11 01:38_

---
