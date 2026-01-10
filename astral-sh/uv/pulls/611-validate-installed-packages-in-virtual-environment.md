```yaml
number: 611
title: Validate installed packages in virtual environment
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/pip-install-verify
created_at: 2023-12-12T02:57:15Z
updated_at: 2023-12-12T17:33:39Z
url: https://github.com/astral-sh/uv/pull/611
synced_at: 2026-01-10T15:44:44Z
```

# Validate installed packages in virtual environment

---

_Pull request opened by @charliermarsh on 2023-12-12 02:57_

## Summary

Now, after running `pip-install`, we validate that the set of installed packages is consistent -- that is, that we don't have any packages that are missing dependencies, or incompatible versions of installed dependencies.


---

_Review requested from @konstin by @charliermarsh on 2023-12-12 02:59_

---

_Review requested from @zanieb by @charliermarsh on 2023-12-12 03:41_

---

_@zanieb approved on 2023-12-12 04:47_

Nice!

---

_Review comment by @konstin on `crates/puffin-installer/src/site_packages.rs`:176 on 2023-12-12 11:12_

```suggestion
                "The package `{package}` requires `{requirement}` but `{version}` is installed."
```
The users shouldn't need to know about the concept of satisfiers

---

_Review comment by @konstin on `crates/puffin-installer/src/site_packages.rs`:97 on 2023-12-12 11:13_

What error message do we get if this fails?

---

_@konstin approved on 2023-12-12 11:13_

---

_Merged by @charliermarsh on 2023-12-12 17:33_

---

_Closed by @charliermarsh on 2023-12-12 17:33_

---

_Branch deleted on 2023-12-12 17:33_

---
