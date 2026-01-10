```yaml
number: 3267
title: fix platform_machine for macos arm
type: pull_request
state: merged
author: Czaki
labels:
  - bug
assignees: []
merged: true
base: main
head: fix_platform_machine_macos
created_at: 2024-04-25T18:12:23Z
updated_at: 2024-04-25T18:17:30Z
url: https://github.com/astral-sh/uv/pull/3267
synced_at: 2026-01-10T14:37:54Z
```

# fix platform_machine for macos arm

---

_Pull request opened by @Czaki on 2024-04-25 18:12_

## Summary

based on PEP 508 the `platform_machine` should be same as `platform.machine()` output: https://peps.python.org/pep-0508/#environment-markers

From my macOS M2 

```python
In [1]: import platform

In [2]: platform.machine()
Out[2]: 'arm64'
```

I napari we also use `arm64` in requirements and it works as expected: 
https://github.com/napari/napari/blob/9fcf63e69ac61b5dff0259c8618f828cc5169a9c/pyproject.toml#L120 

## Test Plan

<!-- How was it tested? -->


---

_@charliermarsh approved on 2024-04-25 18:14_

Thanks, that's my mistake.

---

_Label `bug` added by @charliermarsh on 2024-04-25 18:15_

---

_Merged by @charliermarsh on 2024-04-25 18:16_

---

_Closed by @charliermarsh on 2024-04-25 18:16_

---

_Branch deleted on 2024-04-25 18:17_

---
