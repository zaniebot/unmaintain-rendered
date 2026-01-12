```yaml
number: 5142
title: Warn about unconstrained direct deps in lowest resolution
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/warn-unconstrained-transitive-after-resolution
created_at: 2024-07-17T09:11:46Z
updated_at: 2024-07-18T03:44:54Z
url: https://github.com/astral-sh/uv/pull/5142
synced_at: 2026-01-12T16:06:40Z
```

# Warn about unconstrained direct deps in lowest resolution

---

_@konstin_

Warn when there is a direct dependency without a lower bound and `--resolution lowest` is set.


---

_Label `enhancement` added by @konstin on 2024-07-17 09:11_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:2056 on 2024-07-17 17:46_

```suggestion
                    warn_user_once!("The direct dependency `{package}` is unpinned. Consider setting a lower bound when using `--resolution-strategy lowest` to avoid using outdated versions.");
```

---

_@zanieb reviewed on 2024-07-17 17:46_

---

_@zanieb reviewed on 2024-07-18 03:34_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile.rs`:9553 on 2024-07-18 03:34_

Does this need update still?

---

_@zanieb approved on 2024-07-18 03:34_

---

_Merged by @zanieb on 2024-07-18 03:44_

---

_Closed by @zanieb on 2024-07-18 03:44_

---

_Branch deleted on 2024-07-18 03:44_

---
