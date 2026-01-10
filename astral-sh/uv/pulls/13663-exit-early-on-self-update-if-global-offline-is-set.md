```yaml
number: 13663
title: "Exit early on `self update` if global `--offline` is set"
type: pull_request
state: merged
author: dangdennis
labels:
  - bug
assignees: []
merged: true
base: main
head: dangdennis/offline-prevents-update
created_at: 2025-05-26T17:49:33Z
updated_at: 2025-05-28T02:51:46Z
url: https://github.com/astral-sh/uv/pull/13663
synced_at: 2026-01-10T11:10:42Z
```

# Exit early on `self update` if global `--offline` is set

---

_Pull request opened by @dangdennis on 2025-05-26 17:49_

## Summary
Resolves https://github.com/astral-sh/uv/issues/13580.

`uv self update --offline` should fail and exit early because self-updating requires network connection.

## Test Plan
A snapshot test is added.



---

_Review requested from @Gankra by @konstin on 2025-05-26 20:18_

---

_Label `bug` added by @konstin on 2025-05-26 20:19_

---

_@konstin reviewed on 2025-05-26 20:21_

---

_Review comment by @konstin on `crates/uv/src/commands/self_update.rs`:29 on 2025-05-26 20:21_

To match the Remote Git fetches:

```suggestion
                    "{}{} Self-update is not possible because network connectivity is disabled (i.e., with `--offline`)"
```

---

_Review comment by @konstin on `crates/uv/src/commands/self_update.rs`:32 on 2025-05-26 20:21_

```suggestion
```

---

_@konstin reviewed on 2025-05-26 20:21_

---

_@Gankra approved on 2025-05-27 03:22_

Thanks!

---

_Merged by @Gankra on 2025-05-27 03:29_

---

_Closed by @Gankra on 2025-05-27 03:29_

---

_@dangdennis reviewed on 2025-05-28 02:51_

---

_Review comment by @dangdennis on `crates/uv/src/commands/self_update.rs`:32 on 2025-05-28 02:51_

Thank you @konstin for the help!

---

_Branch deleted on 2025-05-28 02:51_

---
