```yaml
number: 2767
title: "[`flake8-simplify`]: if-to-dict"
type: pull_request
state: merged
author: colin99d
labels: []
assignees: []
merged: true
base: main
head: if-to-dict
created_at: 2023-02-11T15:43:52Z
updated_at: 2023-02-20T20:01:00Z
url: https://github.com/astral-sh/ruff/pull/2767
synced_at: 2026-01-12T15:55:11Z
```

# [`flake8-simplify`]: if-to-dict

---

_@colin99d_

Closes #998 

If this isn't merged tomorrow, I will add more tests. Tests included are the ones that flake8-simplify has.

---

_Review comment by @spaceone on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:77 on 2023-02-11 18:43_

```suggestion
    /// ```python
    /// if x = 1:
```

---

_@spaceone reviewed on 2023-02-11 18:43_

---

_@spaceone reviewed on 2023-02-11 18:44_

---

_Review comment by @spaceone on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:549 on 2023-02-11 18:44_

what if `s` contains a `"`?

---

_Converted to draft by @colin99d on 2023-02-12 20:27_

---

_Marked ready for review by @colin99d on 2023-02-12 20:34_

---

_@colin99d reviewed on 2023-02-12 21:51_

---

_Review comment by @colin99d on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:549 on 2023-02-12 21:51_

Thank you for brining this up. @charliermarsh how do you want me to handle this? Usually we just detect what the type of quotation mark is at the beginning, and use that everywhere. But, this will not work here because if there is a tuple, we will have no way of knowing what the quotation marks were. I say the two options here are:
1. The "lazy" way: just format strings in debug (I implemented this already).
    - Pro: Already implemented, a fixer could switch the quotation marks back
    - Con: All inner double quotations would now be escaped in the users code
2. The "hard" way: write some code to figure out the starting parenthesis for each tuple item.
    - Pro: Users would have strings formatted EXACLTY as they want them
    - Con: Since rust_python does not give us locations for each item, we would need to find ourselves. Which probably involves going to tokens and then back

---

_Comment by @colin99d on 2023-02-12 21:53_

I should note. This PR improves upon the original. In the original the tuple `(1, 2, 3)` would be converted to the string `'1, 2, 3'`. This code preserves the tuple.

---

_@spaceone reviewed on 2023-02-12 23:00_

---

_Review comment by @spaceone on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:549 on 2023-02-12 23:00_

does `unparse_expr(s, checker.stylist)` help? tbh I don't understand what the code does.

---

_Review comment by @colin99d on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:549 on 2023-02-13 01:14_

The issue would be that each item in the tuple could have a different type of quotation mark.

---

_@colin99d reviewed on 2023-02-13 01:14_

---

_@charliermarsh reviewed on 2023-02-13 02:36_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:549 on 2023-02-13 02:36_

We should try to use the `unparse_constant` method of `Generator` for this rather than implementing `constant_to_str` here. But separately, it's fine to pass `checker.stylist` in there and just use the quotation mark that was detected. If the user is mixing quotation marks anyway, it's fine if we modify some of them -- it's kind of like, if the user is mixing indentation styles, we won't respect that either.

(Is there any reason that changing the quotation marks would be a problem, apart from that it might surprise some users?)

---

_@colin99d reviewed on 2023-02-15 01:50_

---

_Review comment by @colin99d on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:549 on 2023-02-15 01:50_

The issue I see is with a tuple like `("hello", 'I say "hi"')` -> `("hello", "I say "hi"")`. And I think this might happen commonly, because if I ever want quotes inside of a string, this is what I would do. I would rather not autofix at all, then have a fix that breaks even 1% of the time. Just let me know what you want me to do!

---

_@charliermarsh reviewed on 2023-02-15 02:53_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:549 on 2023-02-15 02:53_

Will try to review this soon, not tonight but maybe tomorrow. Sorry!

---

_@colin99d reviewed on 2023-02-15 02:54_

---

_Review comment by @colin99d on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:549 on 2023-02-15 02:54_

Your merging an entire auto formatter, so ABSLOTELY no worries!! And, thank you so much for all the hard work. Can't wait until my CI is just:
```
pip install ruff==1.0.0
ruff .
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:111 on 2023-02-15 08:05_

Nit: You can directly use `String::new("Use...")` because you're not using any formatting arguments. 

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:632 on 2023-02-15 08:33_

Nit: You could make use of Rust's pattern matching to remove some of the nesting but it's not necessarily more readable

```suggestion
if let ExprKind::Name { .. } = &left.node {
            if let [Cmpop::Eq] = ops.as_slice() {
                if let [Expr {
                    node: ExprKind::Constant { .. },
                    ..
                }] = comparators.as_slice()
                {
                    if let [Stmt {
                        node: StmtKind::Return { .. },
                        ..
                    }] = body
                    {
                        if let [Stmt {
                            node: StmtKind::If { .. },
                            ..
                        }] = orelse
                        {
                            return true;
                        }
                    }
                }
            }
        }
```

or even

```rust
    if let ExprKind::Compare {
        left,
        ops,
        comparators,
    } = &test.node
    {
        return matches!(
            (
                &left.node,
                ops.as_slice(),
                comparators.as_slice(),
                body,
                orelse
            ),
            (
                ExprKind::Name { .. },
                [Cmpop::Eq],
                [Expr {
                    node: ExprKind::Constant { .. },
                    ..
                }],
                [Stmt {
                    node: StmtKind::Return { .. },
                    ..
                }],
                [Stmt {
                    node: StmtKind::If { .. },
                    ..
                }]
            )
        );
    }
    false
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:671 on 2023-02-15 08:35_

What happens if the byte sequence isn't valid utf-8? I would expect this call to fail. Would it be better to change the return type to `Option<String>` or `Result<SomeError, String>` so that the method can return `None` if the constant cannot be converted to a string?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:730 on 2023-02-15 08:44_

You can avoid the unwrap by using `while let Some(child) = child`

```suggestion
while let Some(current) = child.take() {
        if !should_proceed_child(current, &variable) {
            return;
        }
        if let StmtKind::If { test, body, orelse } = &current.node {
```


---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:734 on 2023-02-15 08:46_

Nit: You can use Rust's let else chain to simplify this code
```suggestion
                let Some(clean_value) = value else { return; };
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:753 on 2023-02-15 08:46_

Nit: Delete?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:777 on 2023-02-15 08:49_

Nit:
```suggestion
            match orelse.as_slice() {
                [Stmt {
                    node: StmtKind::If { .. },
                    ..
                }] => {
                    child = orelse.get(0);
                }
                [Stmt {
                    node: StmtKind::Return { value },
                    ..
                }] => {
                    let final_value = match value {
                        Some(item) => checker
                            .locator
                            .slice_source_code_range(&Range::from_located(item)),
                        None => "None",
                    };
                    else_value = Some(final_value.to_string());
                    child = None;
                }
                [_] => return,
                _ => child = None,
            }
```

---

_@MichaReiser reviewed on 2023-02-15 08:49_

---

_@charliermarsh reviewed on 2023-02-15 13:22_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:111 on 2023-02-15 13:22_

Though it's not obvious, the use of `format!` here is significant as we pick up the `format!` calls and use them to generate documentation on the possible message formats (see: `#[derive_message_formats]`).

---

_Comment by @colin99d on 2023-02-17 01:01_

@charliermarsh I was able to get all the fixes in, and it looks like I was wrong. This handles the quotes correctly! @MichaReiser thank you for all the feedback! The code definitly looks cleaner now.

---

_Comment by @charliermarsh on 2023-02-20 19:09_

(Working on this now.)

---

_Merged by @charliermarsh on 2023-02-20 20:01_

---

_Closed by @charliermarsh on 2023-02-20 20:01_

---
