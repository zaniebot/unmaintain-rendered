```yaml
number: 14615
title: "[pyflakes] Avoid false positives in `@no_type_check` contexts (F821, F722)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
assignees: []
merged: true
base: main
head: brent/fix-13824
created_at: 2024-11-26T17:45:18Z
updated_at: 2024-11-26T19:14:50Z
url: https://github.com/astral-sh/ruff/pull/14615
synced_at: 2026-01-12T15:55:48Z
```

# [pyflakes] Avoid false positives in `@no_type_check` contexts (F821, F722)

---

_@ntBre_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

These changes avoid the false positives reported in #13824, where both F821 (undefined name) and F722 (syntax error in forward annotation) were triggered in function signatures decorated with [typing.no_type_check](https://docs.python.org/3/library/typing.html#typing.no_type_check). In line with the discussion on the issue, the code now skips visiting type definitions when in a context where this decorator was found. While the initial report only covered the function decorator, the docs indicate that classes can also be decorated, so this context is also checked when visiting class definitions, and the new tests reflect that.

## Test Plan

<!-- How was it tested? -->
These changes were tested by including the code snippets triggering the issue as new snapshot tests. As mentioned above, these snapshots are also augmented with `class` variants. I also added invalid annotations to the function and method bodies to make sure the `no_type_check` context applied to the bodies too.


---

_@charliermarsh reviewed on 2024-11-26 17:49_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/mod.rs`:1067 on 2024-11-26 17:49_

I think this should use `match_typing_expr` so that it works for `@typing.no_type_check` too.

---

_@charliermarsh reviewed on 2024-11-26 17:50_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/mod.rs`:1067 on 2024-11-26 17:50_

So, will this apply to type annotations in the _body_ of the function? And should it, per the spec? (I don't know off-hand.)

---

_Comment by @github-actions[bot] on 2024-11-26 17:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-11-26 17:57_

I looked into the `@no_type_check` for red knot and I think we should ignore it for classes, the same as pyright. See https://discuss.python.org/t/no-type-check-decorator/43119. 

Edit: The relevant typing spec change https://github.com/python/typing/pull/1615/files

---

_@ntBre reviewed on 2024-11-26 18:03_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:1067 on 2024-11-26 18:03_

Ah thanks, I missed `match_typing_expr`.

Yes, this should apply to annotations in the body too, based on my reading of the `no_type_check` [docs](https://docs.python.org/3/library/typing.html#typing.no_type_check):
> This works as a class or function [decorator](https://docs.python.org/3/glossary.html#term-decorator). With a class, it applies recursively to all methods and classes defined in that class (but not to methods defined in its superclasses or subclasses). **Type checkers will ignore all annotations in a function or class with this decorator.**

Similarly in the [typing docs](https://typing.readthedocs.io/en/latest/spec/directives.html#no-type-check) linked in what Micha linked:
> If a type checker supports the no_type_check decorator for functions, it should suppress all type errors for the def statement and its body including any nested functions or classes. It should also ignore all parameter and return type annotations and treat the function as if it were unannotated.

I added annotated variables to the test fixtures in my last commit to check this.

---

_Label `rule` added by @MichaReiser on 2024-11-26 18:04_

---

_Comment by @ntBre on 2024-11-26 18:05_

Ah okay, happy to revert the class part. That's what I had initially anyway since that's all that was reported in #13824.

---

_@MichaReiser approved on 2024-11-26 18:48_

Nice. This looks good to me

---

_@charliermarsh approved on 2024-11-26 19:06_

---

_Comment by @MichaReiser on 2024-11-26 19:13_

Thanks

---

_Merged by @MichaReiser on 2024-11-26 19:13_

---

_Closed by @MichaReiser on 2024-11-26 19:13_

---

_Branch deleted on 2024-11-26 19:14_

---
