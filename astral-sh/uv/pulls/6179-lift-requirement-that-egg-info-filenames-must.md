```yaml
number: 6179
title: Lift requirement that .egg-info filenames must include version
type: pull_request
state: merged
author: severen
labels:
  - compatibility
assignees: []
merged: true
base: main
head: optional-egg-info-version
created_at: 2024-08-18T09:59:23Z
updated_at: 2024-08-18T21:20:55Z
url: https://github.com/astral-sh/uv/pull/6179
synced_at: 2026-01-10T13:09:50Z
```

# Lift requirement that .egg-info filenames must include version

---

_Pull request opened by @severen on 2024-08-18 09:59_

## Summary

PR #4533 introduced (almost) spec compliant parsing of `.egg-info` filenames, but added the overly strict requirement that the distribution version must be present. This causes various `uv pip` operations to fail in environments where there are `.egg-info` files without a version component, so loosen this check by making the version component optional and reading the version from the egg metadata when it is not present.

As an example of the issue, running `uv pip list` on my system currently results in
```
error: Failed to read metadata from: `/usr/lib/python3.12/site-packages/PySide6.egg-info`
  Caused by: The `.egg-info` filename "PySide6.egg-info" is missing a version
```
whereas regular `pip list` succeeds:
```
$ pip list | rg -S pyside
PySide6                   6.7.2
```

## Test Plan

This has been tested by altering the `.egg-info` filename tests as needed and ensuring the full test suite passes locally.


---

_Review requested from @charliermarsh by @zanieb on 2024-08-18 13:14_

---

_Assigned to @charliermarsh by @zanieb on 2024-08-18 13:15_

---

_Comment by @zanieb on 2024-08-18 13:15_

Thanks for contributing! This seems reasonable to me, but I'm not the expert on this feature. I've assigned a reviewer and they'll get to this shortly.

---

_@charliermarsh approved on 2024-08-18 17:04_

This looks reasonable to me, thank you.

---

_Label `compatibility` added by @charliermarsh on 2024-08-18 17:04_

---

_Merged by @charliermarsh on 2024-08-18 17:04_

---

_Closed by @charliermarsh on 2024-08-18 17:04_

---

_Branch deleted on 2024-08-18 21:20_

---
