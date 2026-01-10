```yaml
number: 13284
title: "Treat already-installed base environment packages as preferences in `uv run --with`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/non-lock-constraint
created_at: 2025-05-04T15:40:47Z
updated_at: 2025-05-04T23:24:58Z
url: https://github.com/astral-sh/uv/pull/13284
synced_at: 2026-01-10T11:10:41Z
```

# Treat already-installed base environment packages as preferences in `uv run --with`

---

_Pull request opened by @charliermarsh on 2025-05-04 15:40_

## Summary

If a script has some requirements, and you provide `--with`, we currently ignore any constraints from those requirements. We might want to treat them as hard constraints in the future. For now, though, we just treat them as preferences -- so we _prefer_ those versions, but don't require them to match and still run the `--with` resolution in isolation.

Closes https://github.com/astral-sh/uv/issues/13173.


---

_Label `bug` added by @charliermarsh on 2025-05-04 15:40_

---

_Marked ready for review by @charliermarsh on 2025-05-04 15:40_

---

_Review requested from @konstin by @charliermarsh on 2025-05-04 16:56_

---

_Review requested from @zanieb by @charliermarsh on 2025-05-04 16:56_

---

_Review comment by @konstin on `crates/uv/src/commands/project/mod.rs`:1670 on 2025-05-04 21:40_

nit: Should those two be a single method? Otherwise it's hard to spot that one overwrites the other.

---

_Review comment by @konstin on `crates/uv-resolver/src/preferences.rs`:119 on 2025-05-04 21:47_

Note from reviewing: We need this separately from the `InstalledPackagesProvider`  in `CandidateSelector` to have a shared preferences type between preferences from lock and preferences from installed.

---

_@konstin approved on 2025-05-04 21:47_

---

_@charliermarsh reviewed on 2025-05-04 23:16_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/mod.rs`:1670 on 2025-05-04 23:16_

Good call.

---

_Merged by @charliermarsh on 2025-05-04 23:24_

---

_Closed by @charliermarsh on 2025-05-04 23:24_

---

_Branch deleted on 2025-05-04 23:24_

---
