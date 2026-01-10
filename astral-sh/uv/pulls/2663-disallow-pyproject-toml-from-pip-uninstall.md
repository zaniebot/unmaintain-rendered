```yaml
number: 2663
title: "Disallow `pyproject.toml` from `pip uninstall`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/pyproj
created_at: 2024-03-26T00:54:08Z
updated_at: 2024-03-26T01:35:44Z
url: https://github.com/astral-sh/uv/pull/2663
synced_at: 2026-01-10T14:49:08Z
```

# Disallow `pyproject.toml` from `pip uninstall`

---

_Pull request opened by @charliermarsh on 2024-03-26 00:54_

## Summary

Passing `pyproject.toml` or `setup.py` to `pip uninstall` is a bit strange, since it will often require running a resolution to resolve the dependencies (e.g., build the project), which means we also need to accept `--index-url` and friends.


---

_Review requested from @zanieb by @charliermarsh on 2024-03-26 00:54_

---

_Renamed from "Disallow pyproject.toml in pip sync and pip uninstall" to "Disallow `pyproject.toml` in `pip sync` and `pip uninstall`" by @charliermarsh on 2024-03-26 00:54_

---

_Comment by @charliermarsh on 2024-03-26 00:54_

I'm open to allowing it for `pip uninstall` if there's pushback. It's more work and I'll need to wire up all the arguments, but it's doable.

---

_Marked ready for review by @charliermarsh on 2024-03-26 00:54_

---

_@zanieb approved on 2024-03-26 00:56_

`uninstall` seems like a really weird use case..

`sync` seems reasonable if we performed resolution... seems better for a future interface though. 

---

_Renamed from "Disallow `pyproject.toml` in `pip sync` and `pip uninstall`" to "Disallow `pyproject.toml` from `pip uninstall`" by @charliermarsh on 2024-03-26 01:21_

---

_Label `cli` added by @charliermarsh on 2024-03-26 01:21_

---

_Comment by @charliermarsh on 2024-03-26 01:21_

I changed it to just `pip uninstall` for now, since that's what was blocking me elsewhere.

---

_Merged by @charliermarsh on 2024-03-26 01:35_

---

_Closed by @charliermarsh on 2024-03-26 01:35_

---

_Branch deleted on 2024-03-26 01:35_

---
