```yaml
number: 16025
title: "[`flake8-annotations`] Correct syntax for `typing.Union` in suggested return type fixes for `ANN20x` rules"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: typing-union-helper
created_at: 2025-02-07T16:05:20Z
updated_at: 2025-02-07T23:17:20Z
url: https://github.com/astral-sh/ruff/pull/16025
synced_at: 2026-01-12T15:55:53Z
```

# [`flake8-annotations`] Correct syntax for `typing.Union` in suggested return type fixes for `ANN20x` rules

---

_@dylwil3_

When suggesting a return type as a union in Python <=3.9, we now avoid a `TypeError` by correctly suggesting syntax like `Union[int,str,None]` instead of `Union[int | str | None]`.

---

_Label `bug` added by @dylwil3 on 2025-02-07 16:05_

---

_Label `fixes` added by @dylwil3 on 2025-02-07 16:05_

---

_Comment by @github-actions[bot] on 2025-02-07 16:14_

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

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/helpers.rs`:1441 on 2025-02-07 16:26_

I'm not familiar with the code so this is probably a dumb question but the the old implementation applied this function recursively. Is it correct that this is no longer needed? 

---

_@MichaReiser approved on 2025-02-07 16:27_

I starred at the diff for a while but I'm not entirely sure what the root cause or what the fix is. It's probably obvious but I just don't see it 😆 

---

_@dylwil3 reviewed on 2025-02-07 16:45_

---

_Review comment by @dylwil3 on `crates/ruff_python_ast/src/helpers.rs`:1441 on 2025-02-07 16:45_

I was also confused by the code that was there before... At the very least this was a problem:

```rust
            [rest @ .., elt] => Expr::BinOp(ast::ExprBinOp {
                left: Box::new(tuple(rest, binding)),
                op: Operator::BitOr,
                right: Box::new(elt.clone()),
                range: TextRange::default(),
            }),
```

I will try to find an MRE for the difference involving the recursion... but I can't figure out why it would be necessary.

---

_@dylwil3 reviewed on 2025-02-07 18:46_

---

_Review comment by @dylwil3 on `crates/ruff_python_ast/src/helpers.rs`:1441 on 2025-02-07 18:46_

Ok I think the recursion is there just because they were trying to get something like:

```
Union[int | str | float]
```

which is built out of binary operations, so they had to do them one at a time.

But since we're doing:

```
Union[int, str, float]
```

we can just plop down all the elements at once.

The other confusing thing is the treatment of the empty case. An empty `Union` is either a `SyntaxError` (if you do `Union[]`) or a `TypeError` (if you do `Union[()]`). So that's the other difference: the modified function will produce a `SyntaxError` where the previous produced a `TypeError`. But really the function should not be called at all when you have empty `elts`. 

I could:
   - make that the responsibility of the caller
   - change the return signature to `Result`
   - add a `debug_assert`
   - something else?

---

_@dylwil3 reviewed on 2025-02-07 19:08_

---

_Review comment by @dylwil3 on `crates/ruff_python_ast/src/helpers.rs`:1441 on 2025-02-07 19:08_

@charliermarsh if you happen to remember you're reasoning from #8885 and see that I'm missing something, please let me know 😄 

---

_@charliermarsh reviewed on 2025-02-07 22:06_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/helpers.rs`:1441 on 2025-02-07 22:06_

Sorry, it's hard to recall without digging deeper. But, yes, I'm guessing it's something to do with the recursive nature of the `|` operator. Notice that this is very similar to the `pep_604_union` version above.

---

_@dylwil3 reviewed on 2025-02-07 22:59_

---

_Review comment by @dylwil3 on `crates/ruff_python_ast/src/helpers.rs`:1441 on 2025-02-07 22:59_

No worries!

---

_Merged by @dylwil3 on 2025-02-07 23:17_

---

_Closed by @dylwil3 on 2025-02-07 23:17_

---
