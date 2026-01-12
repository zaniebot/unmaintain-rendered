```yaml
number: 7637
title: "Don't suggest replacing `builtin.open()` with `Path.open()` if the latter doesn't support all options"
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - linter
assignees: []
merged: true
base: main
head: fix-gh7620
created_at: 2023-09-25T08:28:33Z
updated_at: 2023-09-26T09:13:36Z
url: https://github.com/astral-sh/ruff/pull/7637
synced_at: 2026-01-12T02:39:10Z
```

# Don't suggest replacing `builtin.open()` with `Path.open()` if the latter doesn't support all options

---

_Pull request opened by @konstin on 2023-09-25 08:28_

**Summary** Check that `closefd` and `opener` aren't being used with `builtin.open()` before suggesting `Path.open()` because pathlib doesn't support these arguments.

Closes #7620

**Test Plan** New cases in the fixture.

---

_Label `bug` added by @konstin on 2023-09-25 08:28_

---

_Label `linter` added by @konstin on 2023-09-25 08:28_

---

_@konstin reviewed on 2023-09-25 08:30_

---

_Review comment by @konstin on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/replaceable_by_pathlib.rs`:108 on 2023-09-25 08:30_

Do we have a `.is_default_value("closefd", 6, Constant::Bool(true))` helper? If no, should i create one?

---

_Comment by @codspeed-hq[bot] on 2023-09-25 08:37_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/fix-gh7620)

### Merging #7637 will **improve performances by 8.46%**

<sub>Comparing <code>fix-gh7620</code> (b64b4a1) with <code>main</code> (39ddad7)</sub>



### Summary

`⚡ 5` improvements
`✅ 20` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `fix-gh7620` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[large/dataset.py]` | 94.1 ms | 90.7 ms | +3.77% |
| ⚡ | `lexer[numpy/ctypeslib.py]` | 2 ms | 1.9 ms | +2.55% |
| ⚡ | `lexer[unicode/pypinyin.py]` | 620.1 µs | 592 µs | +4.74% |
| ⚡ | `lexer[large/dataset.py]` | 9.8 ms | 9 ms | +8.46% |
| ⚡ | `lexer[pydantic/types.py]` | 4.1 ms | 4 ms | +3.69% |


---

_Comment by @github-actions[bot] on 2023-09-25 08:55_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/replaceable_by_pathlib.rs`:107 on 2023-09-25 14:11_

Can these be done with a single match? Like:

```
matches!(expr, Expr::Constant(ast::ExprConstant { value: Constant::Bool(true), .. }))
```

---

_@charliermarsh reviewed on 2023-09-25 14:11_

---

_@charliermarsh reviewed on 2023-09-25 14:11_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/replaceable_by_pathlib.rs`:108 on 2023-09-25 14:11_

I think what you have here is fine personally.

---

_@charliermarsh approved on 2023-09-25 14:11_

---

_Comment by @charliermarsh on 2023-09-25 14:12_

Nice, thanks. Does `Path.open` support all the other kwargs?

---

_Comment by @konstin on 2023-09-26 09:00_

yep, but good reminder to add it to the comment

---

_Merged by @konstin on 2023-09-26 09:07_

---

_Closed by @konstin on 2023-09-26 09:07_

---

_Branch deleted on 2023-09-26 09:07_

---
