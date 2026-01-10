```yaml
number: 10000
title: Preserve multiline strings in code generator
type: pull_request
state: closed
author: sanxiyn
labels: []
assignees: []
draft: true
base: main
head: codegen-multiline-string
created_at: 2024-02-15T14:38:57Z
updated_at: 2024-04-06T16:45:09Z
url: https://github.com/astral-sh/ruff/pull/10000
synced_at: 2026-01-10T22:47:01Z
```

# Preserve multiline strings in code generator

---

_Pull request opened by @sanxiyn on 2024-02-15 14:38_

This is a draft PR for #7799 posted for discussion. Minimally tested with `cargo dev round-trip`.

Extending `ruff_python_literal` to know more about Python string literals (raw, triple, etc.) should be uncontroversial. Changes here are incomplete (no raw, universal newline, bytes...) but completing shouldn't be difficult.

I am less sure about `ruff_python_codegen` changes. AST doesn't have enough information. Instead of adding needed information (after all, it's AST not CST), in this PR `Generator` has an optional `Locator`. When `Locator` is available, string kind information is taken from original source and passed to `ruff_python_literal`. Behavior is unchanged if `Locator` is unavailable.

---

_Comment by @github-actions[bot] on 2024-02-15 14:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2024-03-11 18:05_

@sanxiyn -- in https://github.com/astral-sh/ruff/pull/10298, I modified ruff's AST so that we now track many stylistic details in the AST that were previously not tracked:
- Whether a string is raw or not
- The quoting style (double or single quotes)
- Whether a string is triple-quoted or not

Would you be interested in updating your PR to use the information newly available in our AST? If not, I can pick this up and list you as a co-author

---

_Comment by @MichaReiser on 2024-03-18 10:10_

Using the `Locator` in the `Generator` is clever to avoid tracking additional data in the AST. However, it won't work for AST nodes with no corresponding element in the source (AST nodes that have been built or modified manually). We could dedect such nodes by testing if the range is `0..0`, but I'm somewhat hesitant about making the Generator depend on the `Locator`.

---

_Comment by @charliermarsh on 2024-04-06 16:45_

I'm going to close this PR for now as @AlexWaygood is planning to take a different approach by leveraging the augmented AST.

---

_Closed by @charliermarsh on 2024-04-06 16:45_

---

_Comment by @charliermarsh on 2024-04-06 16:45_

Thanks @sanxiyn!

---
