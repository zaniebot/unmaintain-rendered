```yaml
number: 5201
title: Use +- install output for Python versions
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: charlie/install-message
created_at: 2024-07-19T00:40:53Z
updated_at: 2024-07-19T00:48:36Z
url: https://github.com/astral-sh/uv/pull/5201
synced_at: 2026-01-10T13:42:52Z
```

# Use +- install output for Python versions

---

_Pull request opened by @charliermarsh on 2024-07-19 00:40_

## Summary

Follow-up to https://github.com/astral-sh/uv/pull/4939. Uses a format that's closer to `uv pip install`, with some special-casing for single Pythons.

## Test Plan

A few examples:

![Screenshot 2024-07-18 at 8 39 35 PM](https://github.com/user-attachments/assets/868d4c87-d8f4-4e5f-a52c-1f7a3e8d6a16)
![Screenshot 2024-07-18 at 8 39 46 PM](https://github.com/user-attachments/assets/3a12461e-9d9b-4c33-a685-55ca7256ff52)
![Screenshot 2024-07-18 at 8 39 27 PM](https://github.com/user-attachments/assets/1059e6d6-a445-4531-8496-59bc51d01663)
![Screenshot 2024-07-18 at 8 39 54 PM](https://github.com/user-attachments/assets/dcb69e86-8702-402b-a0cd-6f827b04a6ab)


---

_Review requested from @zanieb by @charliermarsh on 2024-07-19 00:41_

---

_Label `cli` added by @charliermarsh on 2024-07-19 00:41_

---

_Label `preview` added by @charliermarsh on 2024-07-19 00:41_

---

_@charliermarsh reviewed on 2024-07-19 00:41_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/uninstall.rs`:82 on 2024-07-19 00:41_

This ended up feeling unnecessary now that we show the summary at the end.

---

_Review comment by @charliermarsh on `crates/uv-python/src/installation.rs`:353 on 2024-07-19 00:41_

I think this is inverted.

---

_@charliermarsh reviewed on 2024-07-19 00:41_

---

_@zanieb approved on 2024-07-19 00:44_

Gorgeous.

---

_Comment by @charliermarsh on 2024-07-19 00:46_

Thank you!

---

_Merged by @charliermarsh on 2024-07-19 00:46_

---

_Closed by @charliermarsh on 2024-07-19 00:46_

---

_Branch deleted on 2024-07-19 00:46_

---

_@j178 reviewed on 2024-07-19 00:47_

---

_Review comment by @j178 on `crates/uv-python/src/installation.rs`:353 on 2024-07-19 00:47_

Sorry I should've added a explanation here. This is intentional to ensure that Python installations are sorted in descending order by version. https://github.com/astral-sh/uv/issues/5139

---

_@zanieb reviewed on 2024-07-19 00:47_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:353 on 2024-07-19 00:47_

Uh oh! haha 

---

_@charliermarsh reviewed on 2024-07-19 00:48_

---

_Review comment by @charliermarsh on `crates/uv-python/src/installation.rs`:353 on 2024-07-19 00:48_

Hah, I'll take a look.

---
