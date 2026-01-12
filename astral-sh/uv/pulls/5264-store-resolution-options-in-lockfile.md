```yaml
number: 5264
title: Store resolution options in lockfile
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: charlie/save-settings
created_at: 2024-07-21T20:16:02Z
updated_at: 2024-07-22T12:28:24Z
url: https://github.com/astral-sh/uv/pull/5264
synced_at: 2026-01-12T16:06:43Z
```

# Store resolution options in lockfile

---

_@charliermarsh_

## Summary

This PR modifies the lockfile to include the impactful resolution settings, like the resolution and pre-release mode. If any of those values change, we want to ignore the existing lockfile. Otherwise, `--resolution lowest-direct` will typically have no effect, which is really unintuitive.

Closes https://github.com/astral-sh/uv/issues/5226.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-07-21 20:16_

---

_Review requested from @zanieb by @charliermarsh on 2024-07-21 20:16_

---

_Review requested from @konstin by @charliermarsh on 2024-07-21 20:16_

---

_Label `enhancement` added by @charliermarsh on 2024-07-21 20:16_

---

_Label `preview` added by @charliermarsh on 2024-07-21 20:16_

---

_@charliermarsh reviewed on 2024-07-21 20:16_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolution_mode.rs`:28 on 2024-07-21 20:16_

Does anyone know how to get these automatically from the Serde implementation, in `to_toml`?

---

_@charliermarsh reviewed on 2024-07-21 20:17_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:461 on 2024-07-21 20:17_

I decided to omit these if users use the default. It means we may have the "wrong" behavior if we change the default, but I think that's pretty unlikely, and even still, it's kind of fine -- we'd just regenerate their lock. The upside is that we almost never write these, so the lockfile remains succinct.

---

_@konstin approved on 2024-07-22 12:03_

---

_Merged by @charliermarsh on 2024-07-22 12:28_

---

_Closed by @charliermarsh on 2024-07-22 12:28_

---

_Branch deleted on 2024-07-22 12:28_

---
