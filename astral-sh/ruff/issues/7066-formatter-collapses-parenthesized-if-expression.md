```yaml
number: 7066
title: Formatter collapses parenthesized if-expression
type: issue
state: closed
author: cnpryer
labels:
  - formatter
assignees: []
created_at: 2023-09-02T18:35:40Z
updated_at: 2023-11-02T17:01:36Z
url: https://github.com/astral-sh/ruff/issues/7066
synced_at: 2026-01-10T11:09:49Z
```

# Formatter collapses parenthesized if-expression

---

_Issue opened by @cnpryer on 2023-09-02 18:35_

Somewhat related: #6588

Line-length: 88

IMO Ruff's output is better. Reporting anyway.

```py
# Input
class A:
    def b():
        before = (
            # comment
            0
            if self.thing is None
            else before - after
        )

# Black (23.7.0)
class A:
    def b():
        before = (
            # comment
            0
            if self.thing is None
            else before - after
        )

# Ruff (0.0.287)
class A:
    def b():
        before = (
            # comment
            0 if self.thing is None else before - after
        )
```

[Black Playground](https://black.vercel.app/?version=main&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4AEGAI9dAD2IimZxl1N_WlXnON2nzNApqtITJJee2SMSj1y6jsseimwHnaWgdRV97-am_bQdFgonn5VCb4eB0aAvPSsjg1HCaxWkQ3O58HI21wb4GNQYzzg-JaCDBeTFU0L6912zT0sAPADttpSpwfVxrXogAS54zoaeQLKdkY6SM32vDl1YYKz2hSK--Gj9G1LT9z8tAACkVeO1jLhArQABqwGHAgAArEbk_rHEZ_sCAAAAAARZWg==)

[Ruff Playground](https://play.ruff.rs/3a74bf73-09a8-4651-9a02-5f846f4e15be)

---

_Label `formatter` added by @charliermarsh on 2023-09-02 18:56_

---

_Renamed from "Formatter un-expands parenthesized comprehension" to "Formatter un-expands parenthesized if-expression" by @charliermarsh on 2023-09-02 19:00_

---

_Label `good first issue` added by @charliermarsh on 2023-09-02 19:00_

---

_Comment by @cnpryer on 2023-09-02 19:53_

Oh wow I called it a comprehension 🙈 lol. Thanks!

---

_Comment by @charliermarsh on 2023-09-02 20:44_

Oh no prob :) It might also apply to comprehensions, I’m not sure, but I renamed to reflect the example.

---

_Comment by @charliermarsh on 2023-09-02 23:21_

I don't fully understand Black's rules here (some [examples](https://black.vercel.app/?version=main&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4ANMALBdAD2IimZxl1N_WlXnON2nzNApqtITJJee2SMSj1y6jsseimwHnaWgdRV97-am_bQdFgonn5VCb4eB0aAvPSsjg1HCaxWkQ3O58HI21wb4GNQYzzg-JaCDBeTFU0L6914wBjI-YRUYp5hEjBHPvvnSRF_jigCTIfWKNQS_Ei5gb-W9Vlx0guGnm3DI0C5DdrPqsLXPUa5FBtuBDgX-OARI8JjVSrcNCy8KuX4U_xMvKlF6AK1PVEzuuAnUAAHMAc0GAADLabUYscRn-wIAAAAABFla) -- e.g., the first vs. second case is odd and likely a bug given that they drop the comment entirely). The preview style is also quite different.

It might be sufficient to expand the parent if the `IfExp` has leading or trailing own-line comments:

```diff
diff --git a/crates/ruff_python_formatter/src/expression/expr_if_exp.rs b/crates/ruff_python_formatter/src/expression/expr_if_exp.rs
index 1b22fb568..dec9b7d5f 100644
--- a/crates/ruff_python_formatter/src/expression/expr_if_exp.rs
+++ b/crates/ruff_python_formatter/src/expression/expr_if_exp.rs
@@ -53,9 +53,13 @@ impl FormatNodeRule<ExprIfExp> for FormatExprIfExp {
         let comments = f.context().comments().clone();

         let inner = format_with(|f: &mut PyFormatter| {
-            // We place `if test` and `else orelse` on a single line, so the `test` and `orelse` leading
-            // comments go on the line before the `if` or `else` instead of directly ahead `test` or
-            // `orelse`
+            if comments.has_leading(item) || comments.has_trailing(item) {
+                expand_parent().fmt(f)?;
+            }
```

---

_Comment by @charliermarsh on 2023-09-02 23:22_

It would be nice to map this behavior to code in Black to better understand the rules.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-03 15:29_

---

_Label `good first issue` removed by @charliermarsh on 2023-09-03 15:31_

---

_Comment by @charliermarsh on 2023-09-04 08:12_

We decided to accept this as a deviation: https://github.com/astral-sh/ruff/pull/7082

---

_Closed by @charliermarsh on 2023-09-04 08:12_

---

_Renamed from "Formatter un-expands parenthesized if-expression" to "Formatter collapses parenthesized if-expression" by @cnpryer on 2023-11-02 17:01_

---
