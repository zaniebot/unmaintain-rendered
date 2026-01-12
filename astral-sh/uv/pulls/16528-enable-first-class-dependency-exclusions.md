```yaml
number: 16528
title: Enable first-class dependency exclusions
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/exclude
created_at: 2025-10-31T01:06:26Z
updated_at: 2025-12-11T19:51:11Z
url: https://github.com/astral-sh/uv/pull/16528
synced_at: 2026-01-12T16:12:18Z
```

# Enable first-class dependency exclusions

---

_@charliermarsh_

## Summary

This PR adds an `exclude-dependencies` setting that allows users to omit a dependency during resolution. It's effectively a formalized version of the `flask ; python_version < '0'` hack that we've suggested to users in various issues.

Closes #12616.

---

_Review requested from @zanieb by @charliermarsh on 2025-10-31 01:06_

---

_Review requested from @konstin by @charliermarsh on 2025-10-31 01:06_

---

_Label `enhancement` added by @charliermarsh on 2025-10-31 01:06_

---

_Marked ready for review by @charliermarsh on 2025-10-31 01:06_

---

_@zanieb reviewed on 2025-10-31 03:38_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:1871 on 2025-10-31 03:38_

This is the magic

---

_@zanieb approved on 2025-10-31 03:38_

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:1249 on 2025-10-31 09:16_

I'd add something here that excludes are unconditional, the file is use purely as a list of names, as in `anyio; python_version >= "3.12"` will still exclude anyio unconditionally. Similarly, `numpy==2.1.0` will exclude all numpy versions.

---

_Review comment by @konstin on `crates/uv-static/src/env_vars.rs`:138 on 2025-10-31 09:24_

```suggestion
    /// Equivalent to the `--excludes` command-line argument. If set, uv will use this
```

---

_@konstin approved on 2025-10-31 09:26_

:100:

---

_Merged by @charliermarsh on 2025-10-31 14:14_

---

_Closed by @charliermarsh on 2025-10-31 14:14_

---

_Branch deleted on 2025-10-31 14:14_

---

_@ranma42 reviewed on 2025-12-11 19:51_

---

_Review comment by @ranma42 on `crates/uv-scripts/src/lib.rs`:429 on 2025-12-11 19:51_

it looks like this is not handled in RequirementsSpecification.from_pep723_metadata 🤔 

---
