```yaml
number: 21299
title: "Fix main by using `infer_expression`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dhruv/fix-main
created_at: 2025-11-06T16:51:51Z
updated_at: 2025-11-06T20:10:46Z
url: https://github.com/astral-sh/ruff/pull/21299
synced_at: 2026-01-10T16:53:55Z
```

# Fix main by using `infer_expression`

---

_Pull request opened by @dhruvmanila on 2025-11-06 16:51_

_No description provided._

---

_Review requested from @carljm by @dhruvmanila on 2025-11-06 16:51_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-11-06 16:51_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-11-06 16:51_

---

_Label `internal` added by @dhruvmanila on 2025-11-06 16:51_

---

_Review requested from @dcreager by @dhruvmanila on 2025-11-06 16:51_

---

_Label `ty` added by @dhruvmanila on 2025-11-06 16:51_

---

_Comment by @dhruvmanila on 2025-11-06 17:00_

Yikes, the `main` commit fails which is why there are failures in the CI as it tries to build a `ty` executable from `main` as well as this PR.

---

_Comment by @MichaReiser on 2025-11-06 19:49_

Are those checks failing because main isn't building?

---

_@MichaReiser approved on 2025-11-06 19:50_

---

_@dhruvmanila reviewed on 2025-11-06 20:01_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/infer/builder.rs`:4903 on 2025-11-06 20:01_

I think at this point, the expression should've already been inferred because this is in the deferred region and the function symbol should've already been inferred. So, I could directly use `self.expression_type` instead.

---

_Comment by @dhruvmanila on 2025-11-06 20:06_

> Are those checks failing because main isn't building?

Yes!

---

_Comment by @MichaReiser on 2025-11-06 20:10_

Let's merge it. We can easily change it to not use `try` if we feel confident in a follow up PR

---

_Merged by @MichaReiser on 2025-11-06 20:10_

---

_Closed by @MichaReiser on 2025-11-06 20:10_

---

_Branch deleted on 2025-11-06 20:10_

---
