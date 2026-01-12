```yaml
number: 1179
title: "Error when parsing `requirements.txt`-like packages in `requirements.txt` file"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/r
created_at: 2024-01-29T21:27:03Z
updated_at: 2024-01-30T18:55:12Z
url: https://github.com/astral-sh/uv/pull/1179
synced_at: 2026-01-12T16:04:29Z
```

# Error when parsing `requirements.txt`-like packages in `requirements.txt` file

---

_@charliermarsh_

## Summary

Like https://github.com/astral-sh/puffin/pull/1180, this PR adds logic for `requirements.txt` parsing whereby if a requirement _looks like_ a local requirements file or an editable directory, we prompt the user to correct the error (typically, by adding `-r`).


---

_Marked ready for review by @charliermarsh on 2024-01-30 01:49_

---

_Review requested from @zanieb by @charliermarsh on 2024-01-30 01:49_

---

_Review requested from @konstin by @charliermarsh on 2024-01-30 01:49_

---

_Label `enhancement` added by @charliermarsh on 2024-01-30 01:49_

---

_@konstin approved on 2024-01-30 08:58_

---

_Review comment by @zanieb on `crates/requirements-txt/src/lib.rs`:885 on 2024-01-30 15:26_

Do we want to change this too per the discussion in #1180?

Should we just merge both of these and create an issue to follow up?

---

_@zanieb reviewed on 2024-01-30 15:26_

---

_@charliermarsh reviewed on 2024-01-30 15:35_

---

_Review comment by @charliermarsh on `crates/requirements-txt/src/lib.rs`:885 on 2024-01-30 15:35_

Change it to what? Just clarifying, I wasn't sure what the conclusion was. I'll do whatever!

---

_Merged by @charliermarsh on 2024-01-30 18:55_

---

_Closed by @charliermarsh on 2024-01-30 18:55_

---

_Branch deleted on 2024-01-30 18:55_

---
