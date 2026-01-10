```yaml
number: 9365
title: "Surface extras and group conflicts in `uv export`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/default-conflict
created_at: 2024-11-22T19:37:20Z
updated_at: 2024-11-22T20:58:30Z
url: https://github.com/astral-sh/uv/pull/9365
synced_at: 2026-01-10T12:00:00Z
```

# Surface extras and group conflicts in `uv export`

---

_Pull request opened by @charliermarsh on 2024-11-22 19:37_

## Summary

Closes https://github.com/astral-sh/uv/issues/9364.


---

_Label `bug` added by @charliermarsh on 2024-11-22 19:37_

---

_Comment by @BurntSushi on 2024-11-22 19:43_

I'm actually not sure `uv export` can support conflicting groups at all. Or at least, not all cases and not to `requirements.txt`. Because with my ongoing work to fix #9289, some cases can generate ambiguous dependencies. And since PEP 508 markers aren't expressive enough to capture the conditional logic required to disambiguate them, exporting to a flat format like `requirements.txt` that only supports PEP 508 markers could result in a set of requirements where there are multiple versions of the same package.

---

_Comment by @charliermarsh on 2024-11-22 19:44_

Isn't this right, then? We only export a specific set of enabled groups and extras, and this PR ensures that they're all compatible.

---

_Comment by @charliermarsh on 2024-11-22 19:47_

(`uv export` doesn't attempt to export all groups and extras; it exports a specific set of groups and extras.)

---

_@zanieb reviewed on 2024-11-22 20:06_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:2364 on 2024-11-22 20:06_

I know this isn't from this pull request, but can we call these extras something other than `project`, e.g., `extra1` and `extra2`, for clarity? I changed these in the documentation but didn't realize there were tests with the same cases.

---

_@zanieb approved on 2024-11-22 20:06_

---

_@BurntSushi approved on 2024-11-22 20:27_

> Isn't this right, then? We only export a specific set of enabled groups and extras, and this PR ensures that they're all compatible.

Hmmm yeah. I think you're right. False alarm on my part. Sorry!

---

_@charliermarsh reviewed on 2024-11-22 20:28_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:2364 on 2024-11-22 20:28_

Yeah sure.

---

_@charliermarsh reviewed on 2024-11-22 20:58_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:2364 on 2024-11-22 20:58_

Per other PR, I'll do this in a dedicated change, since a lot of other tests do this too.

---

_Merged by @charliermarsh on 2024-11-22 20:58_

---

_Closed by @charliermarsh on 2024-11-22 20:58_

---

_Branch deleted on 2024-11-22 20:58_

---
