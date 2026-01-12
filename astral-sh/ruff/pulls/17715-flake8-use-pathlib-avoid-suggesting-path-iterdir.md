```yaml
number: 17715
title: "[`flake8-use-pathlib`] Avoid suggesting `Path.iterdir()` for `os.listdir` with file descriptor (`PTH208`)"
type: pull_request
state: merged
author: Kalmaegi
labels:
  - bug
assignees: []
merged: true
base: main
head: fix_for_PTH_208
created_at: 2025-04-29T15:59:51Z
updated_at: 2025-04-30T14:40:03Z
url: https://github.com/astral-sh/ruff/pull/17715
synced_at: 2026-01-12T15:56:04Z
```

# [`flake8-use-pathlib`] Avoid suggesting `Path.iterdir()` for `os.listdir` with file descriptor (`PTH208`)

---

_@Kalmaegi_


## Summary

Fixes: #17695

---

_Comment by @github-actions[bot] on 2025-04-29 16:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila requested changes on 2025-04-29 19:29_

Thanks for looking into it. Can you please add soe test cases when `os.listdir` is being used with file descriptors? We should also make sure to support both positional and keyword arguments, so both `os.listdir('/tmp')` and `os.listdir(path='/tmp')` are considered and tested.

---

_@dhruvmanila reviewed on 2025-04-29 19:29_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/replaceable_by_pathlib.rs`:128 on 2025-04-29 19:29_

I think you'd need to use `find_argument_value` or `find_argument` as `os.listdir` accepts the `path` argument as both positional and keyword.

---

_Label `bug` added by @dhruvmanila on 2025-04-29 19:29_

---

_@Kalmaegi reviewed on 2025-04-30 02:46_

---

_Review comment by @Kalmaegi on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/replaceable_by_pathlib.rs`:128 on 2025-04-30 02:46_

Thank you very much for your help! Indeed, I overlooked one scenario.

---

_@dhruvmanila approved on 2025-04-30 14:30_

Thank you!

I think you forgot to update the snapshots which caused the CI failure. I moved the test cases at the end of the file to avoid needing to update the snapshots as this shouldn't raise any diagnostics.

I also took this moment to refactor the code in `replaceable_by_pathlib` to use `let ... else ...` instead of the long `if let ...` code, specifically by separating the `resolve_qualified_name` call and the match against the qualified name. I also update the `diagnostic_kind` conversion to avoid using `Option` and use early return instead.

---

_Renamed from "[`flake8-use-pathlib`] do not suggest Path.iterdir() for os.listdir with file descriptor" to "[`flake8-use-pathlib`] Avoid suggesting `Path.iterdir()` for `os.listdir` with file descriptor (`PTH208`)" by @dhruvmanila on 2025-04-30 14:31_

---

_Comment by @Kalmaegi on 2025-04-30 14:34_

> Thank you!
> 
> I think you forgot to update the snapshots which caused the CI failure. I moved the test cases at the end of the file to avoid needing to update the snapshots as this shouldn't raise any diagnostics.
> 
> I also took this moment to refactor the code in `replaceable_by_pathlib` to use `let ... else ...` instead of the long `if let ...` code, specifically by separating the `resolve_qualified_name` call and the match against the qualified name. I also update the `diagnostic_kind` conversion to avoid using `Option` and use early return instead.

Thank you very much for your help! —I’m also learning a lot from this experience.

---

_Merged by @dhruvmanila on 2025-04-30 14:38_

---

_Closed by @dhruvmanila on 2025-04-30 14:38_

---
