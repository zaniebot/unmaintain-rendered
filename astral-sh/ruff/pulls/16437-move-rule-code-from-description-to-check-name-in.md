```yaml
number: 16437
title: "Move rule code from `description` to `check_name` in GitLab output serializer"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - cli
assignees: []
merged: true
base: main
head: um
created_at: 2025-02-28T11:23:07Z
updated_at: 2025-02-28T14:37:10Z
url: https://github.com/astral-sh/ruff/pull/16437
synced_at: 2026-01-12T15:55:55Z
```

# Move rule code from `description` to `check_name` in GitLab output serializer

---

_@InSyncWithFoo_

## Summary

Resolves #16435.

The changes are summarized by the following table:

| Property      | Has rule? |             Before              |       After        |
|:--------------|:---------:|:-------------------------------:|:------------------:|
| `check_name`  |    Yes    |                -                |       `A123`       |
| `description` |    Yes    |    `A123: Some explanation`     | `Some explanation` |
| `check_name`  |    No     |                -                |   `syntax-error`   |
| `description` |    No     | `SyntaxError: Some explanation` | `Some explanation` |

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-02-28 11:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @InSyncWithFoo on 2025-02-28 11:38_

I don't use GitLab, so this is pretty much guesswork. [The description of `check_name`](https://docs.gitlab.com/ci/testing/code_quality/#code-quality-report-format) says it should be "[a] unique name representing the check, or rule, associated with this violation". Is the rule code enough, or should it be more similar to that of `--output-format=github` (`Ruff (A123)`)?

---

_Comment by @MichaReiser on 2025-02-28 14:01_

The internal name seems fine. This is what eslint does

https://gitlab.com/remcohaszing/eslint-formatter-gitlab/-/blob/main/lib/eslint-formatter-gitlab.js?ref_type=heads#L121

---

_Label `cli` added by @MichaReiser on 2025-02-28 14:01_

---

_@MichaReiser reviewed on 2025-02-28 14:02_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/gitlab.rs`:103 on 2025-02-28 14:02_

Nit: Maybe `syntax-error` to be forward compatible

---

_@MichaReiser approved on 2025-02-28 14:02_

Thanks, this is great

---

_Merged by @MichaReiser on 2025-02-28 14:27_

---

_Closed by @MichaReiser on 2025-02-28 14:27_

---

_Branch deleted on 2025-02-28 14:36_

---
