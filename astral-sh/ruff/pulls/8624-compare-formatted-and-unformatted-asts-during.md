```yaml
number: 8624
title: Compare formatted and unformatted ASTs during formatter tests
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/norm
created_at: 2023-11-12T01:12:10Z
updated_at: 2023-11-27T07:02:23Z
url: https://github.com/astral-sh/ruff/pull/8624
synced_at: 2026-01-10T23:40:55Z
```

# Compare formatted and unformatted ASTs during formatter tests

---

_Pull request opened by @charliermarsh on 2023-11-12 01:12_

## Summary

This PR implements validation in the formatter tests to ensure that we don't modify the AST during formatting. Black has similar logic.

In implementing this, I learned that Black actually _does_ modify the AST, and their test infrastructure normalizes the AST to wipe away those differences. Specifically, Black changes the indentation of docstrings, which _does_ modify the AST; and it also inserts parentheses in `del` statements, which changes the AST too.

Ruff also does both these things, so we _also_ implement the same normalization using a new visitor that allows for modifying the AST.

Closes https://github.com/astral-sh/ruff/issues/8184.

## Test Plan

`cargo test`


---

_@charliermarsh reviewed on 2023-11-12 01:13_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/comparable.rs`:1523 on 2023-11-12 01:13_

(These aren't necessary, but I made the changes before learning that we need to normalize the AST, figured they're worth having anyway.)

---

_@charliermarsh reviewed on 2023-11-12 01:13_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/fixtures.rs`:147 on 2023-11-12 01:13_

Not clear to me why we're doing this, we do this exact thing immediately before the `if`-`else`.

---

_Comment by @github-actions[bot] on 2023-11-12 01:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `formatter` added by @charliermarsh on 2023-11-12 01:33_

---

_Review requested from @zanieb by @charliermarsh on 2023-11-12 17:44_

---

_Review requested from @konstin by @charliermarsh on 2023-11-12 17:44_

---

_Review comment by @konstin on `crates/ruff_python_formatter/tests/fixtures.rs`:285 on 2023-11-13 11:13_

```suggestion
            r#"Reformatting the unformatted code of {} resulted in AST changes. Please open an issue at https://github.com/astral-sh/ruff/issues/new with your configuration and the failing code snippet
```
or something along the lines

---

_@konstin reviewed on 2023-11-13 11:16_

> have to introduce a new Normalized AST specifically for formatting, which is the main downside here (ongoing maintenance whenever we modify the AST).

That's a lot of duplicated code/work/analysis. Could we avoid avoid that by using an AST visitor that applies these change to the cloned AST nodes and then uses the regular comparable AST?

---

_Comment by @charliermarsh on 2023-11-13 14:53_

I think that would require similar boilerplate, would it not? Since we'd need a visitor that's allowed to mutate the AST nodes (which doesn't currently exist).

---

_Comment by @konstin on 2023-11-13 16:19_

That's true, the mutability ruins this

---

_@konstin approved on 2023-11-13 16:19_

---

_Comment by @charliermarsh on 2023-11-13 16:23_

I could do it but we’d need a new visitor, I think. I can at least try.

---

_@charliermarsh reviewed on 2023-11-13 17:37_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/fixtures.rs`:285 on 2023-11-13 17:37_

I think it's fine to omit since this is in tests only.

---

_Merged by @charliermarsh on 2023-11-13 17:43_

---

_Closed by @charliermarsh on 2023-11-13 17:43_

---

_Branch deleted on 2023-11-13 17:43_

---

_@MichaReiser reviewed on 2023-11-27 07:02_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/visitor/transformer.rs`:8 on 2023-11-27 07:02_

Is it important that this modifies the tree in evaluation order or could we use pre-order instead (I was very surprised when I learned that our default visitor visits in evaluation and not pre-order)

---
