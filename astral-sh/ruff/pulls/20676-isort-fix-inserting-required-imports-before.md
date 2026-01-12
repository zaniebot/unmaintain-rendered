```yaml
number: 20676
title: "[`isort`] Fix inserting required imports before future imports (`I002`)"
type: pull_request
state: merged
author: danparizher
labels:
  - rule
  - isort
  - fixes
assignees: []
merged: true
base: main
head: fix-20674
created_at: 2025-10-02T00:00:03Z
updated_at: 2025-10-06T13:44:30Z
url: https://github.com/astral-sh/ruff/pull/20676
synced_at: 2026-01-12T15:57:07Z
```

# [`isort`] Fix inserting required imports before future imports (`I002`)

---

_@danparizher_

## Summary

Fixes #20674


---

_Comment by @github-actions[bot] on 2025-10-02 00:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @amyreese on `crates/ruff_linter/src/importer/mod.rs`:542 on 2025-10-02 00:45_

Since future imports can only be at the beginning, iterating over all statements is inefficient, particularly on large files with many statements. Instead, only look through statements until you've found something that's *not* a future import.

Eg, something like:

```rust
fn find_last_future_import(&self) -> Option<&'a Stmt> {
  self.python_ast.iter().take_while(|stmt| {
    if let Stmt::ImportFrom(import_from) = stmt {
      import_from.module.as_deref() == Some("__future__")
    } else {
      false
    }
  }).last()
}
```

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/isort/snapshots/ruff_linter__rules__isort__tests__required_import_existing_import.py.snap`:8 on 2025-10-02 00:46_

Do you know why these snapshots changed?

---

_@amyreese requested changes on 2025-10-02 00:47_

---

_@danparizher reviewed on 2025-10-02 01:08_

---

_Review comment by @danparizher on `crates/ruff_linter/src/rules/isort/snapshots/ruff_linter__rules__isort__tests__required_import_existing_import.py.snap`:8 on 2025-10-02 01:08_

Before the fix, the required import was being inserted at the very beginning of the file, which would cause a syntax error since future imports must come first.

Now it's inserting the required import after the existing future import, which maintains the correct order and prevents the syntax error.

---

_Review requested from @amyreese by @danparizher on 2025-10-02 01:09_

---

_@amyreese approved on 2025-10-02 15:38_

---

_Label `rule` added by @amyreese on 2025-10-02 15:40_

---

_Label `isort` added by @amyreese on 2025-10-02 15:40_

---

_Label `fixes` added by @amyreese on 2025-10-02 15:40_

---

_Review requested from @ntBre by @amyreese on 2025-10-02 15:46_

---

_Review comment by @ntBre on `crates/ruff_linter/src/importer/mod.rs`:545 on 2025-10-02 17:45_

It looks like this won't properly account for docstrings:

```shell
> ./target/debug/ruff check --select I --config 'lint.isort.required-imports=["import this"]' try.py --preview --no-cache
I002 [*] Missing required import: `import this`
--> try.py:1:1
help: Insert required import: `import this`
1 | "a docstring"
2 + import this
3 | from __future__ import annotations

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

I also noticed by looking at the `Insertion::start_of_file` references that we have this `preceding_import` function, which accesses import information held on the `Importer`:

https://github.com/astral-sh/ruff/blob/4e94b22815c7979003d07a4931c9174285617c90/crates/ruff_linter/src/importer/mod.rs#L509-L515

Can we use the `runtime_imports` field referenced here instead of iterating over the statements directly? It seems functionally the same, but maybe a bit more clear what's going on.

This preceded the PR, but it looks like the `add_import` docs are a bit out of date too. This part doesn't seem to be true:

```rust
    /// Otherwise, it will be added after the most recent top-level
    /// import statement.
```

Rather, the import is added after the import preceding whatever you pass as the `at` argument. I think I002 would actually work fine if the docstring were accurate. So an alternative way to fix this would be to pass a more accurate `at` in the I002 rule code. That actually seems kind of tempting to me instead of modifying the general helper.

---

_@ntBre requested changes on 2025-10-02 18:07_

I think we need to handle docstrings, and I also had some ideas for other places to add this check.

---

_@danparizher reviewed on 2025-10-02 22:24_

---

_Review comment by @danparizher on `crates/ruff_linter/src/importer/mod.rs`:545 on 2025-10-02 22:24_

The `runtime_imports` approach would require populating it in the I002 rule, but since the rule doesn't call `visit_import()`, it would be empty. Modifying the general helper seemed cleaner since it fixes the issue for all users of `add_import`, not just I002.

```rust
.take_while(|stmt| {
    match stmt {
        Stmt::ImportFrom(import_from) => import_from.module.as_deref() == Some("__future__"),
        Stmt::Expr(stmt_expr) => matches!(stmt_expr.value.as_ref(), Expr::StringLiteral(_)),
        _ => false,
    }
})
```


---

_Review requested from @ntBre by @danparizher on 2025-10-02 22:29_

---

_Review comment by @ntBre on `crates/ruff_linter/src/importer/mod.rs`:558 on 2025-10-03 14:14_

I think we could take some inspiration from the `match_docstring_end` helper and just skip one statement. It's not a huge deal since something like this is a separate syntax error, but it still seems a bit more precise:

```py
"docstring"
"another string"
from __future__ import annotations  # SyntaxError: from __future__ imports must occur at the beginning of the file
```

What do you think about something like this?

```rust
    /// Find the last valid `from __future__` import statement in the AST.
    fn find_last_future_import(&self) -> Option<&'a Stmt> {
        let mut body = self.python_ast.iter().peekable();
        let _docstring = body.next_if(|stmt| is_docstring_stmt(stmt));

        body.take_while(|stmt| {
            stmt.as_import_from_stmt()
                .is_some_and(|import_from| import_from.module.as_deref() == Some("__future__"))
        })
        .last()
    }
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/isort/mod.rs`:1040 on 2025-10-03 14:15_

Can we make these two `test_case`s on the same test function? The bodies of the functions look pretty much the same to me, besides the snapshot name. I think using the test case path is enough differentiation in the filename, though.

---

_@ntBre reviewed on 2025-10-03 14:15_

---

_Review comment by @ntBre on `crates/ruff_linter/src/importer/mod.rs`:540 on 2025-10-06 13:35_

```suggestion
        let mut body = self.python_ast.iter().peekable();
        let _docstring = body.next_if(|stmt| ast::helpers::is_docstring_stmt(stmt));
```

---

_@ntBre approved on 2025-10-06 13:36_

Thank you!

---

_Merged by @ntBre on 2025-10-06 13:40_

---

_Closed by @ntBre on 2025-10-06 13:40_

---

_Branch deleted on 2025-10-06 13:43_

---
