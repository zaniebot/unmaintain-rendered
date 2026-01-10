```yaml
number: 608
title: Respect existing versions when pip-installing
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/pip-install-preferences
created_at: 2023-12-11T19:50:05Z
updated_at: 2023-12-12T17:22:49Z
url: https://github.com/astral-sh/uv/pull/608
synced_at: 2026-01-10T15:44:44Z
```

# Respect existing versions when pip-installing

---

_Pull request opened by @charliermarsh on 2023-12-11 19:50_

## Summary

When running `puffin pip-install`, we should respect versions that are already installed in the environment. For example, if you run `puffin pip-install flask==2.0.0` and then `puffin pip-install flask`, we should avoid upgrading Flask. The most natural way to model this is to mark them as "preferences".

(It's not enough to just filter those requirements out prior to resolving, since we may not have the _dependencies_ of those packages installed. We _could_ recursively verify this across the `site-packages`, but that would be a larger PR.)


---

_Review requested from @zanieb by @charliermarsh on 2023-12-12 03:41_

---

_Review requested from @konstin by @charliermarsh on 2023-12-12 03:41_

---

_@zanieb approved on 2023-12-12 04:53_

---

_Review comment by @konstin on `crates/puffin-installer/src/site_packages.rs`:40 on 2023-12-12 10:28_

I expect we'll eventually grow our own https://docs.python.org/3/library/importlib.metadata.html

---

_@konstin approved on 2023-12-12 10:29_

Good test cases!

---

_Merged by @charliermarsh on 2023-12-12 17:22_

---

_Closed by @charliermarsh on 2023-12-12 17:22_

---

_Branch deleted on 2023-12-12 17:22_

---
