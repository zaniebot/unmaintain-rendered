```yaml
number: 14815
title: "Improve error messages for except* (B025, B029, B030, B904) #14791"
type: pull_request
state: merged
author: smokyabdulrahman
labels:
  - diagnostics
assignees: []
merged: true
base: main
head: fix/various-rules-refer-to-except-star-as-except-14791
created_at: 2024-12-06T12:40:18Z
updated_at: 2024-12-08T17:38:59Z
url: https://github.com/astral-sh/ruff/pull/14815
synced_at: 2026-01-12T15:55:49Z
```

# Improve error messages for except* (B025, B029, B030, B904) #14791

---

_@smokyabdulrahman_

Improves error message for [except*](https://peps.python.org/pep-0654/) (Rules: B025, B029, B030, B904)

Example python snippet:
```python
try:
    a = 1
except* ValueError:
    a = 2
except* ValueError:
    a = 2

try:
    pass
except* ():
    pass

try:
    pass
except* 1:  # error
    pass

try:
    raise ValueError
except* ValueError:
    raise UserWarning
```
Error messages
Before:
```
$ ruff check --select=B foo.py
foo.py:6:9: B025 try-except block with duplicate exception `ValueError`
foo.py:11:1: B029 Using `except ():` with an empty tuple does not catch anything; add exceptions to handle
foo.py:16:9: B030 `except` handlers should only be exception classes or tuples of exception classes
foo.py:22:5: B904 Within an `except` clause, raise exceptions with `raise ... from err` or `raise ... from None` to distinguish them from errors in exception handling
Found 4 errors.
```
After:
```
$ ruff check --select=B foo.py
foo.py:6:9: B025 try-except* block with duplicate exception `ValueError`
foo.py:11:1: B029 Using `except* ():` with an empty tuple does not catch anything; add exceptions to handle
foo.py:16:9: B030 `except*` handlers should only be exception classes or tuples of exception classes
foo.py:22:5: B904 Within an `except*` clause, raise exceptions with `raise ... from err` or `raise ... from None` to distinguish them from errors in exception handling
Found 4 errors.
```

Closes https://github.com/astral-sh/ruff/issues/14791

---

_@charliermarsh reviewed on 2024-12-06 12:51_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/duplicate_exceptions.rs`:57 on 2024-12-06 12:51_

For obscure reasons, let's do this like:
```rust
if is_star {
  format!("try-except* block with duplicate exception `{name}`")
} else {
  format!("try-except block with duplicate exception `{name}`")
}
```

(These messages get included as-is in the docs, so it's preferable to expand them like that sometimes.)

---

_Comment by @charliermarsh on 2024-12-06 12:51_

If you're able, can you run `cargo test -p ruff_linter --lib` and then `cargo insta accept` to update the checked-in snapshots? You'll need to run `cargo install cargo-insta` first.

---

_Comment by @smokyabdulrahman on 2024-12-06 13:01_

> If you're able, can you run `cargo test -p ruff_linter --lib` and then `cargo insta accept` to update the checked-in snapshots? You'll need to run `cargo install cargo-insta` first.

Yes, sure thing. I've ran the tests locally before committing. However, I didn't know how to accept my test modifications.

Thank you 👌 I will be doing that with in the next commit.

---

_@smokyabdulrahman reviewed on 2024-12-06 13:02_

---

_Review comment by @smokyabdulrahman on `crates/ruff_linter/src/rules/flake8_bugbear/rules/duplicate_exceptions.rs`:57 on 2024-12-06 13:02_

sure thing 👍 
I will adopt this approach with all the rules.

---

_@charliermarsh approved on 2024-12-06 18:53_

---

_Comment by @smokyabdulrahman on 2024-12-06 19:00_

BTW @charliermarsh 

Since I am going to fix a couple more rules also. (B025, B029, B030, B904)

Does it make sense to do the following change [in the statement parser](https://github.com/astral-sh/ruff/blob/2119dcab6ffb89613b0be4d40dc68b5a2f01526a/crates/ruff_python_parser/src/parser/statement.rs#L1549):
```rust
            ExceptHandler::ExceptHandler(ast::ExceptHandlerExceptHandler {
                type_,
                name,
                body: except_body,
                range: self.node_range(start),
                in_trystar: block_kind.is_star(), <---- `try` block kind
            }),
```

so that `in_trystar` is accessible in all the rules handlers.
or is that an overkill?

---

_Comment by @charliermarsh on 2024-12-06 19:00_

@smokyabdulrahman -- I'd prefer not to change the parser / AST, if possible.

---

_Comment by @github-actions[bot] on 2024-12-06 19:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @smokyabdulrahman on 2024-12-06 20:40_

---

_Renamed from "WIP: Improve error messages for except* #14791" to "Improve error messages for except* (B025, B029, B030, B904) #14791" by @smokyabdulrahman on 2024-12-06 20:42_

---

_Label `diagnostics` added by @dylwil3 on 2024-12-06 23:00_

---

_@MichaReiser reviewed on 2024-12-07 18:28_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast_integration_tests/tests/visitor.rs`:250 on 2024-12-07 18:28_

I'd prefer if we can keep the `Visitor` trait unchanged. 

You can access the `try` statement from inside the rules using:

```
let is_star = checker
                    .semantic()
                    .current_statement()
                    .as_try_stmt()
                    .is_some_and(|try_stmt| try_stmt.is_star);
```

---

_@smokyabdulrahman reviewed on 2024-12-07 18:30_

---

_Review comment by @smokyabdulrahman on `crates/ruff_python_ast_integration_tests/tests/visitor.rs`:250 on 2024-12-07 18:30_

Got it 👍 
I will be applying the changes.

---

_@MichaReiser reviewed on 2024-12-07 18:37_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast_integration_tests/tests/visitor.rs`:250 on 2024-12-07 18:37_

Thanks

---

_@smokyabdulrahman reviewed on 2024-12-07 19:05_

---

_Review comment by @smokyabdulrahman on `crates/ruff_python_ast_integration_tests/tests/visitor.rs`:250 on 2024-12-07 19:05_

Change applied.
It's much simpler now!

I should spend sometime reading the common structs' definitions next time. :+1:

---

_Review requested from @MichaReiser by @smokyabdulrahman on 2024-12-08 17:06_

---

_@MichaReiser approved on 2024-12-08 17:33_

Thank you!

---

_@smokyabdulrahman reviewed on 2024-12-08 17:34_

---

_Review comment by @smokyabdulrahman on `crates/ruff_linter/src/rules/flake8_bugbear/rules/duplicate_exceptions.rs`:219 on 2024-12-08 17:34_

Just for learning purposes. @MichaReiser 
Why was this moved inside the `diagnostic-branch`?

I thought calculating it outside the loop once would be better.

---

_Merged by @MichaReiser on 2024-12-08 17:37_

---

_Closed by @MichaReiser on 2024-12-08 17:37_

---
