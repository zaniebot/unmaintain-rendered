```yaml
number: 2537
title: "Search in both `purelib` and `platlib` for site-packages population"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/site-packages
created_at: 2024-03-19T02:53:18Z
updated_at: 2024-03-19T03:06:17Z
url: https://github.com/astral-sh/uv/pull/2537
synced_at: 2026-01-12T16:05:05Z
```

# Search in both `purelib` and `platlib` for site-packages population

---

_@charliermarsh_

## Summary

In reality, there's no such thing as the `site-packages` directory for a given virtualenv. Rather, Python defines both `purelib` and `platlib`, where the former is for pure-Python packages and the latter is for packages that contain native code. These are almost always set to the same thing... but they don't _have_ to be, and in fact of Fedora they are not.

This PR changes the `site_packages` method to return an iterator of directories.

Note that the system tests introduced in https://github.com/astral-sh/uv/pull/2535, which failed on the PR itself, now pass.

Closes https://github.com/astral-sh/uv/issues/2527.


---

_Label `bug` added by @charliermarsh on 2024-03-19 02:53_

---

_Marked ready for review by @charliermarsh on 2024-03-19 02:53_

---

_Merged by @charliermarsh on 2024-03-19 03:06_

---

_Closed by @charliermarsh on 2024-03-19 03:06_

---

_Branch deleted on 2024-03-19 03:06_

---
