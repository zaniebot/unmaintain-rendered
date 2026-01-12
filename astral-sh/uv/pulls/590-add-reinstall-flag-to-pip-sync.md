```yaml
number: 590
title: "Add `--reinstall` flag to `pip-sync`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/rebuild
created_at: 2023-12-08T02:33:14Z
updated_at: 2023-12-08T19:58:44Z
url: https://github.com/astral-sh/uv/pull/590
synced_at: 2026-01-12T16:04:03Z
```

# Add `--reinstall` flag to `pip-sync`

---

_@charliermarsh_

## Summary

This PR adds two flags to `pip-sync`: `--reinstall`, and `--reinstall-package [PACKAGE]`. The former reinstalls all packages in the requirements, while the latter can be repeated and reinstalls all specified packages.

For our purposes, a reinstall includes (1) purging the cache, and (2) marking any already-installed versions as extraneous.

Closes #572.

Closes https://github.com/astral-sh/puffin/issues/271.

---

_Review requested from @zanieb by @charliermarsh on 2023-12-08 02:33_

---

_Review requested from @konstin by @charliermarsh on 2023-12-08 02:33_

---

_Label `enhancement` added by @charliermarsh on 2023-12-08 02:33_

---

_Marked ready for review by @charliermarsh on 2023-12-08 02:33_

---

_Review comment by @konstin on `crates/puffin-cli/src/main.rs`:188 on 2023-12-08 08:54_

Personal preference: `--reinstall` and `--reinstall-all`

---

_@konstin approved on 2023-12-08 08:54_

---

_@zanieb reviewed on 2023-12-08 14:51_

---

_Review comment by @zanieb on `crates/puffin-cli/src/main.rs`:188 on 2023-12-08 14:51_

I think the first thing I'd do in that case is `pip sync --reinstall` then be confused when I'm prompted for a specific package. 

---

_@charliermarsh reviewed on 2023-12-08 19:57_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/main.rs`:188 on 2023-12-08 19:57_

Yeah I'm torn. Hard to know in advance what users will expect here.

---

_Merged by @charliermarsh on 2023-12-08 19:58_

---

_Closed by @charliermarsh on 2023-12-08 19:58_

---

_Branch deleted on 2023-12-08 19:58_

---
