```yaml
number: 9900
title: "Reduce size of `Expr` from 80 to 64 bytes"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/box
created_at: 2024-02-08T20:02:10Z
updated_at: 2024-02-10T01:16:25Z
url: https://github.com/astral-sh/ruff/pull/9900
synced_at: 2026-01-12T15:55:30Z
```

# Reduce size of `Expr` from 80 to 64 bytes

---

_@charliermarsh_

## Summary

This PR reduces the size of `Expr` from 80 to 64 bytes, by reducing the sizes of...

- `ExprCall` from 72 to 56 bytes, by using boxed slices for `Arguments`.
- `ExprCompare` from 64 to 48 bytes, by using boxed slices for its various vectors.

In testing, the parser gets a bit faster, and the linter benchmarks improve quite a bit.


---

_Comment by @codspeed-hq[bot] on 2024-02-08 20:28_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/box)

### Merging #9900 will **improve performances by 4.49%**

<sub>Comparing <code>charlie/box</code> (708ef73) with <code>main</code> (bd8123c)</sub>



### Summary

`⚡ 1` improvements
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/box` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-with-preview-rules[large/dataset.py]` | 100.2 ms | 95.9 ms | +4.49% |


---

_Comment by @charliermarsh on 2024-02-08 20:29_

That's a bit better...

---

_Comment by @charliermarsh on 2024-02-08 20:29_

I think I can do even better though.

---

_Comment by @charliermarsh on 2024-02-08 21:02_

Okay, there's no longer any parser regression, and there are parser and linter improvements across the board (though many don't meet our threshold).

---

_Review requested from @BurntSushi by @charliermarsh on 2024-02-08 21:06_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-02-08 21:06_

---

_Marked ready for review by @charliermarsh on 2024-02-08 21:08_

---

_Label `performance` added by @charliermarsh on 2024-02-08 21:09_

---

_@charliermarsh reviewed on 2024-02-08 21:17_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/nodes.rs`:2991 on 2024-02-08 21:17_

We could make these `Option<Box<...>>` to avoid allocating when empty...

---

_@charliermarsh reviewed on 2024-02-08 21:17_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/nodes.rs`:898 on 2024-02-08 21:17_

These are never empty so zero-length slice is not a problem.

---

_@charliermarsh reviewed on 2024-02-08 21:17_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/nodes.rs`:3935 on 2024-02-08 21:17_

@MichaReiser - I missed your comment when we talked in-person -- did you want me to include these as `assert!`?

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/flake8_pyi/rules/bad_version_info_comparison.rs`:72 on 2024-02-08 21:44_

You're a champ.

---

_Review comment by @BurntSushi on `crates/ruff_python_parser/src/function.rs`:140 on 2024-02-08 21:47_

I might suggest adding a comment explaining what's going on here.

---

_@BurntSushi approved on 2024-02-08 21:47_

Master of the double-star.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/bad_version_info_comparison.rs`:72 on 2024-02-09 02:44_

We're no longer allowed to use `as_slice`... 😨 😛 

---

_Merged by @charliermarsh on 2024-02-09 02:53_

---

_Closed by @charliermarsh on 2024-02-09 02:53_

---

_Branch deleted on 2024-02-09 02:53_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:2991 on 2024-02-09 02:59_

It should be possible to use `Option` without increasing the struct size, and avoiding the allocation could be nice. although It doesn't seem that the empty slice allocates 

https://godbolt.org/z/E9cxqE9P3 CC: @BurntSushi 

---

_@MichaReiser approved on 2024-02-09 02:59_

---

_Comment by @github-actions[bot] on 2024-02-09 03:03_

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

_@charliermarsh reviewed on 2024-02-09 03:14_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/nodes.rs`:2991 on 2024-02-09 03:14_

Gasp...

---

_@MichaReiser reviewed on 2024-02-09 03:18_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:3935 on 2024-02-09 03:18_

Ah sorry. I don't mind keeping them. I do find them useful when analyzing the type's size in the future.

---

_@BurntSushi reviewed on 2024-02-10 01:16_

---

_Review comment by @BurntSushi on `crates/ruff_python_ast/src/nodes.rs`:2991 on 2024-02-10 01:16_

Yeah that is interesting. I actually couldn't find how this was being done in std specifically. So it could just be an optimization in the code generator somewhere. Or perhaps I missed a call to [`dangling`](https://doc.rust-lang.org/std/ptr/struct.NonNull.html#method.dangling).

---
