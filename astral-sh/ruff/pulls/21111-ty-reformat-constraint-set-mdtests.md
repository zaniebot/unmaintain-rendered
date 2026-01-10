```yaml
number: 21111
title: "[ty] Reformat constraint set mdtests"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/refactor-constraint-mdtests
created_at: 2025-10-28T17:52:26Z
updated_at: 2025-10-28T18:59:51Z
url: https://github.com/astral-sh/ruff/pull/21111
synced_at: 2026-01-10T16:59:49Z
```

# [ty] Reformat constraint set mdtests

---

_Pull request opened by @dcreager on 2025-10-28 17:52_

This PR updates the mdtests that test how our generics solver interacts with our new constraint set implementation. Because the rendering of a constraint set can get long, this standardizes on putting the `revealed` assertion on a separate line. We also add a `static_assert` test for each constraint set to verify that they are all coerced into simple `bool`s correctly.

This is a pure reformatting (not even a refactoring!) that changes no behavior. I've pulled it out of #20093 to reduce the amount of effort that will be required to review that PR.

---

_Review requested from @carljm by @dcreager on 2025-10-28 17:52_

---

_Review requested from @AlexWaygood by @dcreager on 2025-10-28 17:52_

---

_Review requested from @sharkdp by @dcreager on 2025-10-28 17:52_

---

_Label `internal` added by @dcreager on 2025-10-28 17:52_

---

_Label `ty` added by @dcreager on 2025-10-28 17:52_

---

_@MichaReiser approved on 2025-10-28 18:09_

---

_Comment by @dcreager on 2025-10-28 18:37_

Rebased this onto `main` — it doesn't actually depend on anything in #21068, and since it's got a :heavy_check_mark: I'll go ahead and merge this

---

_Merged by @dcreager on 2025-10-28 18:59_

---

_Closed by @dcreager on 2025-10-28 18:59_

---

_Branch deleted on 2025-10-28 18:59_

---
