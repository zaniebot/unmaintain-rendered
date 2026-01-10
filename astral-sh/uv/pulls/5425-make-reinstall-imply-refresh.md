```yaml
number: 5425
title: "Make `--reinstall` imply `--refresh`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: charlie/refresh-reinstall
created_at: 2024-07-24T21:10:57Z
updated_at: 2024-08-23T15:49:17Z
url: https://github.com/astral-sh/uv/pull/5425
synced_at: 2026-01-10T13:09:50Z
```

# Make `--reinstall` imply `--refresh`

---

_Pull request opened by @charliermarsh on 2024-07-24 21:10_

## Summary

It's hard for me to imagine a scenario in which a user passed `--reinstall`, but wanted us to keep respecting cached data for a package. For example, to actually "rebuild and reinstall" an editable today, you have to pass both `--reinstall` and `--refresh`.

This PR makes `--reinstall` imply `--refresh`, so we always validate that the cached data is fresh.

Closes https://github.com/astral-sh/uv/issues/5424.


---

_Label `enhancement` added by @charliermarsh on 2024-07-24 21:11_

---

_Label `cli` added by @charliermarsh on 2024-07-24 21:11_

---

_Review requested from @zanieb by @charliermarsh on 2024-07-24 21:11_

---

_Review requested from @konstin by @charliermarsh on 2024-07-24 21:11_

---

_Comment by @charliermarsh on 2024-07-24 21:13_

I guess it's still confusing that `--refresh foo` doesn't trigger a reinstall of `foo` if `foo` is editable.

---

_Comment by @charliermarsh on 2024-07-24 21:13_

But still an improvement.

---

_@zanieb approved on 2024-07-24 21:14_

---

_@konstin approved on 2024-07-25 09:55_

That simplifies the different between `--reinstall` and `--refresh` a lot

---

_Merged by @charliermarsh on 2024-07-25 13:45_

---

_Closed by @charliermarsh on 2024-07-25 13:45_

---

_Branch deleted on 2024-07-25 13:46_

---
