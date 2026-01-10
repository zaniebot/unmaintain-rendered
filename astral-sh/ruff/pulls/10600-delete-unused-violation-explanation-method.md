```yaml
number: 10600
title: "Delete unused `Violation::explanation` method"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: delete-unused-explanation-method
created_at: 2024-03-26T07:42:40Z
updated_at: 2024-03-27T05:59:25Z
url: https://github.com/astral-sh/ruff/pull/10600
synced_at: 2026-01-10T22:47:02Z
```

# Delete unused `Violation::explanation` method

---

_Pull request opened by @MichaReiser on 2024-03-26 07:42_

## Summary

Deletes the unused `Violation::explanation` method

## Test Plan

`cargo build`


---

_Comment by @github-actions[bot] on 2024-03-26 07:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `internal` added by @MichaReiser on 2024-03-26 07:58_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-03-26 07:58_

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/violation.rs`:28 on 2024-03-26 07:59_

Let's see in how many years I have time to work on this :laughing: 

---

_@MichaReiser reviewed on 2024-03-26 07:59_

---

_@AlexWaygood approved on 2024-03-26 11:26_

---

_Merged by @MichaReiser on 2024-03-26 11:35_

---

_Closed by @MichaReiser on 2024-03-26 11:35_

---

_Branch deleted on 2024-03-26 11:35_

---

_Comment by @dhruvmanila on 2024-03-26 12:44_

Nice! Does this mean that not adding a documentation is a compiler error?

---

_Comment by @MichaReiser on 2024-03-26 12:58_

> Nice! Does this mean that not adding a documentation is a compiler error?

That I don't know. I only removed the `explanation` method. I assume that the explanation was never used during runtime. `cargo dev generate-all` must store it somewhere else.

---

_Comment by @dhruvmanila on 2024-03-26 13:13_

Yeah, the `violation` macro generates the `explanation` method only if there's a doc comment on the struct. It does give me a compiler error if there's no such method because there's no default implementation now:

```
rustc: function or associated item `explanation` not found for this struct
     crates/ruff_linter/src/codes.rs(65:1): original diagnostic [E0599]
```

---

_Comment by @MichaReiser on 2024-03-26 13:14_

I guess that's a nice added benefit :D Although it might be annoying when adding a new rule

Does that mean that we never called the method using the trait directly.

---

_Comment by @dhruvmanila on 2024-03-27 05:59_

> Does that mean that we never called the method using the trait directly.

Yeah, I don't think so. The macro must be directly calling it like `rule.explanation()` 🤷‍♂️ 

---
