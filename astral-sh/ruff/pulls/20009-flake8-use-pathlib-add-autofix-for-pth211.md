```yaml
number: 20009
title: "[`flake8-use-pathlib`] Add autofix for `PTH211`"
type: pull_request
state: merged
author: chirizxc
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: pth211-autofix
created_at: 2025-08-20T19:29:11Z
updated_at: 2025-08-22T18:37:11Z
url: https://github.com/astral-sh/ruff/pull/20009
synced_at: 2026-01-10T17:46:21Z
```

# [`flake8-use-pathlib`] Add autofix for `PTH211`

---

_Pull request opened by @chirizxc on 2025-08-20 19:29_

## Summary
Part of https://github.com/astral-sh/ruff/issues/2331
## Test Plan
`cargo nextest run flake8_use_pathlib`

---

_Comment by @github-actions[bot] on 2025-08-20 20:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_symlink.rs`:128 on 2025-08-22 15:15_

Can you add a test where there is some whitespace around `False`?

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_symlink.rs`:81 on 2025-08-22 15:23_

It looks like we don't offer a fix when `dir_fd = None`, though - was that intentional? (It's probably okay not to)

---

_@dylwil3 requested changes on 2025-08-22 15:23_

Thanks for this! Couple small comments.

---

_@chirizxc reviewed on 2025-08-22 15:39_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_symlink.rs`:81 on 2025-08-22 15:39_

In other autofixes, we have the same `if is_keyword_only_argument_non_default(&call.arguments, <...>)`

---

_@chirizxc reviewed on 2025-08-22 15:43_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_symlink.rs`:81 on 2025-08-22 15:43_

[os_rmdir.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_rmdir.rs), [os_rmdir.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_rmdir.rs) and other files



---

_Comment by @chirizxc on 2025-08-22 16:02_

done


---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_symlink.rs`:128 on 2025-08-22 16:26_

It looks like you added a test with whitespace around `True`. I was hoping for a test like this:

```python
os.symlink("usr/bin/python",  dst="tmp/python", target_is_directory=     False    )
```

to see whether your expression range that you're slicing includes the whitespace or not. 

In fact: I think it'd probably be better to just replace this code snippet with something like:

```rust
            .and_then(|expr| {
                let code = locator.slice(expr.range());
                expr.as_boolean_literal_expr()
                    .is_some_and(|bl| bl.value == true)
                    .then(|| format!(", target_is_directory={code}"))
            })
```

---

_@dylwil3 reviewed on 2025-08-22 16:26_

---

_@chirizxc reviewed on 2025-08-22 16:44_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_symlink.rs`:128 on 2025-08-22 16:44_

<img width="1338" height="487" alt="изображение" src="https://github.com/user-attachments/assets/308cc8a3-6ea1-4f03-9d90-b3e1162e8e18" />
with these spaces also works without replacing it with this code

---

_@chirizxc reviewed on 2025-08-22 16:48_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_symlink.rs`:128 on 2025-08-22 16:48_

<img width="1187" height="153" alt="изображение" src="https://github.com/user-attachments/assets/cc65afc7-3898-4e1f-9e24-036ec1446ddd" />
In this case, there are also invalid fixes for the variant with `target_is_directory=True`

---

_@dylwil3 approved on 2025-08-22 17:07_

Thank you!

---

_Label `fixes` added by @dylwil3 on 2025-08-22 17:07_

---

_Label `preview` added by @dylwil3 on 2025-08-22 17:07_

---

_Comment by @dylwil3 on 2025-08-22 17:29_

Adding a minor edit - it looks like you can pass an arbitrary type to `target_is_directory` without CPython throwing an error. (I think it evaluates the truthiness of the value). I've skipped the fix in these cases instead of just throwing out the value, since otherwise I think we may change program behavior. If CI passes I'll merge

---

_Comment by @chirizxc on 2025-08-22 17:38_

For some reason, a lot of `Safe fixes` are missing from the snapshots

---

_Merged by @dylwil3 on 2025-08-22 17:38_

---

_Closed by @dylwil3 on 2025-08-22 17:38_

---

_Comment by @chirizxc on 2025-08-22 17:39_

<img width="1777" height="784" alt="изображение" src="https://github.com/user-attachments/assets/7a35e156-6930-4024-9b1d-82e0a681eedf" />
i mean this

---

_Comment by @dylwil3 on 2025-08-22 18:31_

> For some reason, a lot of `Safe fixes` are missing from the snapshots

I swear I checked the ecosystem results and everything... maybe they were not updated even when they said they were? Oy. Thank you for the quick followup PR!

---

_Branch deleted on 2025-08-22 18:37_

---
