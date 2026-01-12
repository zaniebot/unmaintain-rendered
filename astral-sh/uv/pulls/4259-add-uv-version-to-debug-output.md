```yaml
number: 4259
title: Add uv version to debug output
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/debug
created_at: 2024-06-12T00:50:11Z
updated_at: 2024-06-12T02:59:31Z
url: https://github.com/astral-sh/uv/pull/4259
synced_at: 2026-01-12T16:06:07Z
```

# Add uv version to debug output

---

_@charliermarsh_

## Summary

I think this is a useful piece of connective tissue that will let us avoid back-and-forths when folks include traces.

## Test Plan

```
❯ cargo run pip list --verbose
DEBUG uv 0.2.11 (44041bccd 2024-06-11)
DEBUG Searching for Python interpreter in virtual environments
DEBUG Found CPython 3.12.3 at `/Users/crmarsh/workspace/puffin/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.12.3 environment at .venv/bin/python3
```


---

_Review requested from @zanieb by @charliermarsh on 2024-06-12 02:03_

---

_Label `cli` added by @charliermarsh on 2024-06-12 02:03_

---

_@zanieb approved on 2024-06-12 02:50_

---

_Merged by @charliermarsh on 2024-06-12 02:59_

---

_Closed by @charliermarsh on 2024-06-12 02:59_

---

_Branch deleted on 2024-06-12 02:59_

---
