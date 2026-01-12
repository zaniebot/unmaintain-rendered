```yaml
number: 21177
title: "[`pylint`] Fix `PLR1708` false positives on nested functions"
type: pull_request
state: merged
author: chirizxc
labels:
  - bug
  - rule
  - preview
assignees: []
merged: true
base: main
head: plr1708
created_at: 2025-10-31T21:59:35Z
updated_at: 2025-11-21T20:51:29Z
url: https://github.com/astral-sh/ruff/pull/21177
synced_at: 2026-01-12T15:57:18Z
```

# [`pylint`] Fix `PLR1708` false positives on nested functions

---

_@chirizxc_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/21162

## Test Plan

`cargo nextest run pylint`


---

_Comment by @github-actions[bot] on 2025-10-31 22:12_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Label `bug` added by @ntBre on 2025-11-06 19:30_

---

_Label `preview` added by @ntBre on 2025-11-06 19:30_

---

_Label `rule` added by @ntBre on 2025-11-06 19:30_

---

_@ntBre requested changes on 2025-11-06 19:44_

Thanks for working on this, but I think the fix here is too simplistic, and we actually need to be traversing scopes. As one example, this code triggers the runtime error in CPython but is no longer flagged by the rule:

```pycon
>>> def h():
...     yield 1
...     class C:
...         raise StopIteration
...     yield C
...
... for i in h(): ...
...
Ellipsis
Traceback (most recent call last):
  File "<python-input-0>", line 3, in h
    class C:
        raise StopIteration
  File "<python-input-0>", line 4, in C
    raise StopIteration
StopIteration

The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "<python-input-0>", line 7, in <module>
    for i in h(): ...
             ~^^
RuntimeError: generator raised StopIteration
```

I'm wondering if we should just run the rule on `StmtFunctionDef`s instead of `raise` statements and then check for both `yield` and `raise StopIteration` while visiting the function body, but I haven't thought about it much more than that.

---

_Review requested from @ntBre by @chirizxc on 2025-11-14 13:35_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/stop_iteration_return.rs`:64 on 2025-11-18 21:07_

nit:
```suggestion
    analyzer.visit_body(&function_def.body);
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/stop_iteration_return.rs`:108 on 2025-11-18 21:24_

I'm not sure how helpful this helper function is. I think we could just destructure the `StmtRaise` in the `match` on line 83 and use `map_callable`:

https://github.com/astral-sh/ruff/blob/192c37d5401786b3043aafbfb657b4f5e7df8ac8/crates/ruff_python_ast/src/helpers.rs#L724-L735

For example,

```rust
    fn visit_stmt(&mut self, stmt: &'a ast::Stmt) {
        match stmt {
            ast::Stmt::FunctionDef(_) => {}
            ast::Stmt::Raise(ast::StmtRaise { exc: Some(exc), .. }) => {
                if self.checker.semantic().match_builtin_expr(map_callable(exc), "StopIteration") {
                    self.stop_iteration_raises.push(stmt_raise);
                }
                walk_stmt(self, stmt);
            }
            _ => walk_stmt(self, stmt),
        }
    }
```

I was going to suggest removing the `walk_stmt` call in the `Stmt::Raise` arm, but I think that's actually correct, and B901 has a bug in it by only checking for statements with a single yield expression:

https://github.com/astral-sh/ruff/blob/192c37d5401786b3043aafbfb657b4f5e7df8ac8/crates/ruff_linter/src/rules/flake8_bugbear/rules/return_in_generator.rs#L118-L123

That means it misses cases like this:

```py
def foo():
    return (yield 1)
```

Feel free to follow up on that if you're interested!

This also applies here, where your code will correctly flag something like this:

```py
def foo():
    raise StopIteration((yield 1))
```

That might be a nice test case to add.

---

_@ntBre reviewed on 2025-11-18 21:29_

Thanks! This looks good to me now, I just had a couple more minor simplification suggestions.

---

_Review requested from @ntBre by @chirizxc on 2025-11-21 14:19_

---

_@ntBre approved on 2025-11-21 20:40_

Thank you!

---

_Renamed from "[`pylint`] Fix `PLR1708` false positives" to "[`pylint`] Fix `PLR1708` false positives on nested functions" by @ntBre on 2025-11-21 20:40_

---

_Merged by @ntBre on 2025-11-21 20:41_

---

_Closed by @ntBre on 2025-11-21 20:41_

---

_Branch deleted on 2025-11-21 20:51_

---
