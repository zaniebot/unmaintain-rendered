```yaml
number: 2632
title: "Limit overrides and constraints to `requirements.txt` format"
type: pull_request
state: merged
author: charliermarsh
labels:
  - breaking
assignees: []
merged: true
base: main
head: charlie/setup-i
created_at: 2024-03-23T02:02:41Z
updated_at: 2024-03-23T13:33:30Z
url: https://github.com/astral-sh/uv/pull/2632
synced_at: 2026-01-12T16:05:08Z
```

# Limit overrides and constraints to `requirements.txt` format

---

_@charliermarsh_

## Summary

I don't see a great reason to allow this, and it adds a lot of complexity, so `pyproject.toml` files are now limited to `pip compile` and `pip install -r` -- they can't be passed as `-c` or `--override`.


---

_Label `breaking` added by @charliermarsh on 2024-03-23 02:02_

---

_Marked ready for review by @charliermarsh on 2024-03-23 02:02_

---

_@zanieb approved on 2024-03-23 02:38_

---

_Merged by @charliermarsh on 2024-03-23 13:33_

---

_Closed by @charliermarsh on 2024-03-23 13:33_

---

_Branch deleted on 2024-03-23 13:33_

---
