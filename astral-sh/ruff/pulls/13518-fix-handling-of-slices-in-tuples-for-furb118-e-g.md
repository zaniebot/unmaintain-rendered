```yaml
number: 13518
title: "Fix handling of slices in tuples for FURB118, e.g., `x[:, 1]`"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/furb118-fix
created_at: 2024-09-26T00:02:30Z
updated_at: 2024-09-26T14:29:16Z
url: https://github.com/astral-sh/ruff/pull/13518
synced_at: 2026-01-10T20:59:36Z
```

# Fix handling of slices in tuples for FURB118, e.g., `x[:, 1]`

---

_Pull request opened by @zanieb on 2024-09-26 00:02_

There was already handling for the singleton `x[:]` case but not the tuple case.

Closes https://github.com/astral-sh/ruff/issues/13508


---

_Label `bug` added by @zanieb on 2024-09-26 00:02_

---

_Comment by @github-actions[bot] on 2024-09-26 00:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/refurb/rules/reimplemented_operator.rs`:224 on 2024-09-26 01:06_

Presumably we'll drop some trivia and fixes won't retain verbatim spacing and such? We could consider only doing this if a `:` is in the elements?

---

_@zanieb reviewed on 2024-09-26 01:06_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/reimplemented_operator.rs`:224 on 2024-09-26 06:27_

Nit: `Itertools::join`
```suggestion
            .join(", ");
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/reimplemented_operator.rs`:224 on 2024-09-26 06:33_

I think that would be nice.


---

_@MichaReiser approved on 2024-09-26 06:34_

Thanks

---

_@AlexWaygood reviewed on 2024-09-26 14:10_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/reimplemented_operator.rs`:217 on 2024-09-26 14:10_

nit: you can iterate over the tuple directly

```suggestion
```

---

_Merged by @zanieb on 2024-09-26 14:20_

---

_Closed by @zanieb on 2024-09-26 14:20_

---

_Branch deleted on 2024-09-26 14:20_

---
