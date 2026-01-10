```yaml
number: 5849
title: "Ensure `python`-to-`pythonX.Y` symlink exists in downloaded Pythons"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/sym
created_at: 2024-08-07T02:49:41Z
updated_at: 2024-08-07T03:03:23Z
url: https://github.com/astral-sh/uv/pull/5849
synced_at: 2026-01-10T13:31:54Z
```

# Ensure `python`-to-`pythonX.Y` symlink exists in downloaded Pythons

---

_Pull request opened by @charliermarsh on 2024-08-07 02:49_

## Summary

After installing:

```
❯ readlink "/Users/crmarsh/Library/Application Support/uv/python/cpython-3.12.4-macos-aarch64-none/bin/python"
python3.12

❯ readlink "/Users/crmarsh/Library/Application Support/uv/python/cpython-3.9.5-macos-aarch64-none/bin/python"
python3.9
```

Closes https://github.com/astral-sh/uv/issues/5838.


---

_Label `bug` added by @charliermarsh on 2024-08-07 02:49_

---

_Label `preview` added by @charliermarsh on 2024-08-07 02:49_

---

_Review requested from @zanieb by @charliermarsh on 2024-08-07 02:49_

---

_@zanieb approved on 2024-08-07 02:54_

---

_Merged by @charliermarsh on 2024-08-07 03:03_

---

_Closed by @charliermarsh on 2024-08-07 03:03_

---

_Branch deleted on 2024-08-07 03:03_

---
