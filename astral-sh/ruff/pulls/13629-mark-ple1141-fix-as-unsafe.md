```yaml
number: 13629
title: "Mark `PLE1141` fix as unsafe"
type: pull_request
state: merged
author: zanieb
labels:
  - rule
  - fixes
assignees: []
merged: true
base: main
head: zb/ple1141-unsafe
created_at: 2024-10-04T15:36:28Z
updated_at: 2024-10-04T19:23:14Z
url: https://github.com/astral-sh/ruff/pull/13629
synced_at: 2026-01-12T15:55:45Z
```

# Mark `PLE1141` fix as unsafe

---

_@zanieb_

Closes https://github.com/astral-sh/ruff/issues/13343

---

_Label `rule` added by @zanieb on 2024-10-04 15:36_

---

_Label `fixes` added by @zanieb on 2024-10-04 15:36_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pylint/rules/dict_iter_missing_items.rs`:47 on 2024-10-04 15:37_

See also #13627 

---

_@zanieb reviewed on 2024-10-04 15:37_

---

_Comment by @github-actions[bot] on 2024-10-04 15:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pylint/rules/dict_iter_missing_items.rs`:46 on 2024-10-04 18:56_

```suggestion
/// The tuple key is unpacked into `x` and `y` instead of the key and values. This means that
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pylint/rules/dict_iter_missing_items.rs`:48 on 2024-10-04 18:56_

```suggestion
/// cannot infer the likely type of a dictionary's keys.
```

---

_@AlexWaygood approved on 2024-10-04 18:56_

---

_@zanieb reviewed on 2024-10-04 19:08_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pylint/rules/dict_iter_missing_items.rs`:48 on 2024-10-04 19:08_

```suggestion
/// cannot consistently infer the type of a dictionary's keys.
```

---

_@AlexWaygood approved on 2024-10-04 19:14_

---

_Merged by @zanieb on 2024-10-04 19:22_

---

_Closed by @zanieb on 2024-10-04 19:22_

---

_Branch deleted on 2024-10-04 19:22_

---
