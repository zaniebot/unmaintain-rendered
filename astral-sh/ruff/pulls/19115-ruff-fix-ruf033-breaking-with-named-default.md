```yaml
number: 19115
title: "[`ruff`] Fix `RUF033` breaking with named default expressions"
type: pull_request
state: merged
author: robsdedude
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix/18950-ruf033-named-expression-as-default
created_at: 2025-07-03T07:39:48Z
updated_at: 2025-07-24T14:44:56Z
url: https://github.com/astral-sh/ruff/pull/19115
synced_at: 2026-01-10T17:58:13Z
```

# [`ruff`] Fix `RUF033` breaking with named default expressions

---

_Pull request opened by @robsdedude on 2025-07-03 07:39_

## Summary
The generated fix for `RUF033` would cause a syntax error for named expressions as parameter defaults.
```python
from dataclasses import InitVar, dataclass
@dataclass
class Foo:
    def __post_init__(self, bar: int = (x := 1)) -> None:
        pass
```
would be turned into
```python
from dataclasses import InitVar, dataclass
@dataclass
class Foo:
    x: InitVar[int] = x := 1
    def __post_init__(self, bar: int = (x := 1)) -> None:
        pass
```
instead of the syntactically correct
```python
# ...
x: InitVar[int] = (x := 1)
# ...
```

## Test Plan
Test reproducer (plus some extra tests) have been added to the test suite.

## Related
Fixes: https://github.com/astral-sh/ruff/issues/18950

---

_Comment by @github-actions[bot] on 2025-07-03 07:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @robsdedude on 2025-07-03 08:26_

---

_Label `bug` added by @ntBre on 2025-07-04 20:05_

---

_Label `fixes` added by @ntBre on 2025-07-04 20:05_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/post_init_default.rs`:157 on 2025-07-04 20:13_

This check looks kind of convoluted and unnecessary to me. I don't think the `ParameterWithDefault` AST node is likely to change. That's basically what it's checking right?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/post_init_default.rs`:164 on 2025-07-04 20:19_

I don't think we need to switch all the way to using the code generator here. Can't we just check if the expression `is_expr_named` and add parens if so? Or I think we could even check the `parenthesized_range` and preserve the parens it came with.

It's nice to prefer range replacements because the generator strips out any comments, as well as being much more verbose.

---

_@ntBre reviewed on 2025-07-04 20:21_

Thanks! I think the results look right, but I have a couple of suggestions for the implementation.

---

_@robsdedude reviewed on 2025-07-04 22:43_

---

_Review comment by @robsdedude on `crates/ruff_linter/src/rules/ruff/rules/post_init_default.rs`:157 on 2025-07-04 22:43_

Yes. In particular it's asserting that no other syntactic element is added after the parameter's default value to make sure that the following deletion does not accidentally end up deleting something else but the default value.

I obviously can't give you an example of what such an element would look like because I don't have a crystal ~~ball~~ _snake_, but think of type hints that have been added and would've made a similar assumption invalid: if anything follows after a parameter name, it's its default value. 

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/post_init_default.rs`:157 on 2025-07-09 18:53_

I still think this is an overly defensive check and not _really_ related to the issue at hand. I imagine it would be pretty clear that we should re-evaluate any use of `ParameterWithDefault` if the Python grammar changes, so I'd rather not clutter the rule implementation.

---

_@ntBre reviewed on 2025-07-09 18:53_

---

_@robsdedude reviewed on 2025-07-22 17:59_

---

_Review comment by @robsdedude on `crates/ruff_linter/src/rules/ruff/rules/post_init_default.rs`:164 on 2025-07-22 17:59_

I managed to rewrite it using `parenthesized_range`. It's passing the same tests. But if feels crazy fragile. Looking at the code I cannot convince myself of its correctness and that I haven't missed any edge case. I don't feel comfortable with my name being attached to that code. Sorry.

---

_Closed by @robsdedude on 2025-07-22 17:59_

---

_@ntBre reviewed on 2025-07-22 18:07_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/post_init_default.rs`:164 on 2025-07-22 18:07_

If you feel that strongly about it, we can keep the old implementation. I don't really see how it's more fragile, and we have similar checks in other rules, but this was only a slight preference on my part, not a blocker. I'd much rather fix the bug and give you credit for your work here. I'll approve and merge this if you want to revert/drop the last commit and reopen the PR.

---

_Review comment by @robsdedude on `crates/ruff_linter/src/rules/ruff/rules/post_init_default.rs`:164 on 2025-07-23 07:36_

Where I think this is fragile: if the Syntax ever changes, string replacements like these bake in too many assumptions. That's how this bug happened in the first place: a new syntax construct (walrus operator) was introduced and the replacement logic wasn't up to the task.

Moreover, `parenthesized_range` calls `parentheses_iterator`, which in turn is documented with 
```rust
/// Returns an iterator over the ranges of the optional parentheses surrounding an expression.
```
Yeah no... these parens are surely not optional :grimacing: It's quite unfortunate, I find, that the AST does not always include the parens where they're necessary in the expression's range. But neither does Python's own `ast` module :shrug: 

I slept a night over it and I'll re-open the PR. If not, I would've given you permission to take my code and commit it under your name. So regardless of how this ends _some_ fix can be merged.

---

_@robsdedude reviewed on 2025-07-23 07:37_

---

_Reopened by @robsdedude on 2025-07-23 07:37_

---

_@ntBre reviewed on 2025-07-23 14:33_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/post_init_default.rs`:164 on 2025-07-23 14:33_

> Where I think this is fragile: if the Syntax ever changes, string replacements like these bake in too many assumptions. That's how this bug happened in the first place: a new syntax construct (walrus operator) was introduced and the replacement logic wasn't up to the task.

Ahh, I see where you're coming from. I think that explains the other assertion too. I guess I wasn't really thinking about it in the same way since the walrus operator and Python 3.8 predate Ruff by ~3 years. So it wasn't so much that something new came out and broke us, just that the initial rule implementation didn't account for everything that already existed.

> Yeah no... these parens are surely not optional

I can also see what you mean here, but I always read this as referring to a Rust `Option`, like the return type of `parenthesized_range`, as in the expression may or may not have parentheses rather than may or may not require parentheses.

Anyway, I'm happy with either implementation. I'll give this another quick review now.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/ruff/RUF033.py`:116 on 2025-07-23 14:35_

Can we use a different name here? It's funny, but I think we try to avoid this kind of thing.

---

_@ntBre approved on 2025-07-23 14:39_

Thanks again! Just one nit about a test case name, but this is otherwise good to go from my side.

---

_@ntBre approved on 2025-07-24 13:06_

Thank you! I'll merge after the patch release finishes.

---

_Merged by @ntBre on 2025-07-24 13:45_

---

_Closed by @ntBre on 2025-07-24 13:45_

---

_Branch deleted on 2025-07-24 14:44_

---
