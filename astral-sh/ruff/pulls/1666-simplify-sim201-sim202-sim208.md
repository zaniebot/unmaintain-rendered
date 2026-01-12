```yaml
number: 1666
title: Simplify SIM201, SIM202, SIM208
type: pull_request
state: merged
author: chammika-become
labels: []
assignees: []
merged: true
base: main
head: simplify
created_at: 2023-01-05T17:35:24Z
updated_at: 2023-01-06T17:05:55Z
url: https://github.com/astral-sh/ruff/pull/1666
synced_at: 2026-01-12T05:36:32Z
```

# Simplify SIM201, SIM202, SIM208

---

_Pull request opened by @chammika-become on 2023-01-05 17:35_

Flake8 simplify #998 

SIM201, SIM202 and SIM208 is done here with fixes.

Note: SIM203 == E713 

@charliermarsh Please let me know what changes are required to accept this

---

_Review comment by @charliermarsh on `src/flake8_simplify/plugins/unary_ops.rs`:107 on 2023-01-05 18:40_

We have a helper called `create_expr` that you could use here, in `ast/helpers.rs`.We have a helper called `create_expr` that you could use here, in `ast/helpers.rs`.

---

_Review comment by @charliermarsh on `src/flake8_simplify/plugins/unary_ops.rs`:90 on 2023-01-05 18:40_

Nit: remove space from `SIM 208`.

---

_Review comment by @charliermarsh on `src/flake8_simplify/plugins/unary_ops.rs`:39 on 2023-01-05 18:43_

I know it's a little tedious but we should avoid calling `.to_string()` on expressions and statements.

Instead we should _either_ grab them with `SourceCodeLocator` (`checker.locator`) or round-trip them with `SourceCodeGenerator` to ensure that they're in the proper style. I've been preferring the latter for the SIM checks. Like:

```rs
let mut generator = SourceCodeGenerator::new(
    checker.style.indentation(),
    checker.style.quote(),
    checker.style.line_ending(),
);
generator.unparse_expr(&left, 0);
generator.generate()
```

---

_Review comment by @charliermarsh on `src/flake8_simplify/plugins/unary_ops.rs`:79 on 2023-01-05 18:44_

Can we change this to a `match`, and log the error?

We have this kind of pattern in the codebase, if you want to grep for some examples:

```rust
match fixes::fix_unnecessary_literal_set(locator, expr) {
    Ok(fix) => {
        check.amend(fix);
    }
    Err(e) => error!("Failed to generate fix: {e}"),
}
```

---

_Review comment by @charliermarsh on `src/flake8_simplify/plugins/unary_ops.rs`:118 on 2023-01-05 18:44_

Same here regarding logging the error.

---

_Review comment by @charliermarsh on `src/flake8_simplify/plugins/unary_ops.rs`:43 on 2023-01-05 18:44_

Same here regarding logging the error.

---

_@charliermarsh requested changes on 2023-01-05 18:44_

This looks great! Just some minor comments.

---

_Review comment by @charliermarsh on `resources/test/fixtures/flake8_simplify/SIM201.py`:10 on 2023-01-05 18:44_

What's `NG` stand for here?

---

_@charliermarsh reviewed on 2023-01-05 18:44_

---

_@charliermarsh reviewed on 2023-01-05 18:45_

---

_Review comment by @charliermarsh on `resources/test/fixtures/flake8_simplify/SIM201_2.py`:1 on 2023-01-05 18:45_

Why is this special-cased? I'm assuming flake8-simplify special-cases it, just wondering what the reasoning is.

---

_Comment by @charliermarsh on 2023-01-05 18:45_

Thanks @chammika-become! Left a few comments. Heads up that I merged in `main`.

---

_Review comment by @chammika-become on `src/flake8_simplify/plugins/unary_ops.rs`:39 on 2023-01-06 10:51_

Thanks, ended up moving above to `fn expr_with_style` and use it when generating styled `String`s for `Checks` and `Fixes`. 

---

_@chammika-become reviewed on 2023-01-06 10:51_

---

_@chammika-become reviewed on 2023-01-06 10:53_

---

_Review comment by @chammika-become on `src/flake8_simplify/plugins/unary_ops.rs`:79 on 2023-01-06 10:53_

Done. Use the updated `compare` from the latest.

---

_@chammika-become reviewed on 2023-01-06 10:55_

---

_Review comment by @chammika-become on `resources/test/fixtures/flake8_simplify/SIM201.py`:10 on 2023-01-06 10:55_

NG - Not Good cases.
I will remove it if it's confusing, I am just used to NG as an opposite to OK.

---

_@chammika-become reviewed on 2023-01-06 11:04_

---

_Review comment by @chammika-become on `resources/test/fixtures/flake8_simplify/SIM201_2.py`:1 on 2023-01-06 11:04_

It's in `flake8_simplify` 
code : [SIM201](https://github.com/MartinThoma/flake8-simplify/blob/d3cb9556a5255969f83a8329df8f635da41f595d/flake8_simplify/rules/ast_unary_op.py#L24-L25)
test: [not in exception check](https://github.com/MartinThoma/flake8-simplify/blob/d3cb9556a5255969f83a8329df8f635da41f595d/tests/test_200_rules.py#L10-L15)

I was wondering too, but implement it faithful to the original code. 

---

_Marked ready for review by @chammika-become on 2023-01-06 11:18_

---

_Review requested from @charliermarsh by @chammika-become on 2023-01-06 14:44_

---

_@charliermarsh reviewed on 2023-01-06 15:39_

---

_Review comment by @charliermarsh on `resources/test/fixtures/flake8_simplify/SIM201_2.py`:1 on 2023-01-06 15:39_

Here's the PR that introduced it: https://github.com/MartinThoma/flake8-simplify/issues/13

No context given, but probably correct to preserve it for compatibility as you did here.

---

_@charliermarsh reviewed on 2023-01-06 15:46_

---

_Review comment by @charliermarsh on `resources/test/fixtures/flake8_simplify/SIM201.py`:10 on 2023-01-06 15:46_

I changed it to `OK`, is that wrong? Since these cases are "OK", in that they don't trigger errors? Let me now if I've misunderstood, and I'll correct in a follow-up commit.

---

_Merged by @charliermarsh on 2023-01-06 15:47_

---

_Closed by @charliermarsh on 2023-01-06 15:47_

---

_Comment by @charliermarsh on 2023-01-06 15:48_

Thank you for this! Great PR!

---

_@TomFryers reviewed on 2023-01-06 17:05_

---

_Review comment by @TomFryers on `resources/test/fixtures/flake8_simplify/SIM201_2.py`:1 on 2023-01-06 17:05_

My guess would be that the reason to keep the `not` is to maintain the parallel `if not cond: raise ...` has to `assert cond`.

---
