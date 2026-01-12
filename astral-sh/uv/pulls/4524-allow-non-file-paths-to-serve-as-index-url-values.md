```yaml
number: 4524
title: "Allow non-file:// paths to serve as `--index-url` values"
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
assignees: []
merged: true
base: main
head: charlie/local-index
created_at: 2024-06-25T17:23:52Z
updated_at: 2024-06-25T17:57:13Z
url: https://github.com/astral-sh/uv/pull/4524
synced_at: 2026-01-12T16:06:17Z
```

# Allow non-file:// paths to serve as `--index-url` values

---

_@charliermarsh_

## Summary

pip allows these with the following logic:

```python
if os.path.exists(location):  # Is a local path.
    url = path_to_url(location)
    path = location
elif location.startswith("file:"):  # A file: URL.
    url = location
    path = url_to_path(location)
elif is_url(location):
    url = location
```

Closes https://github.com/astral-sh/uv/issues/4510.

## Test Plan

`cargo run pip install --index-url ../packse/index/simple-html/ example-a-961b4c22 --reinstall --no-cache --no-deps`


---

_Marked ready for review by @charliermarsh on 2024-06-25 17:23_

---

_Renamed from "Allow non-file:// paths to serve as --index-url values" to "Allow non-file:// paths to serve as `--index-url` values" by @charliermarsh on 2024-06-25 17:24_

---

_Label `compatibility` added by @charliermarsh on 2024-06-25 17:24_

---

_Merged by @charliermarsh on 2024-06-25 17:57_

---

_Closed by @charliermarsh on 2024-06-25 17:57_

---

_Branch deleted on 2024-06-25 17:57_

---
