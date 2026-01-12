```yaml
number: 7067
title: "Formatter collapses overlong assignment's parenthesized value"
type: issue
state: closed
author: cnpryer
labels:
  - formatter
assignees: []
created_at: 2023-09-02T18:50:37Z
updated_at: 2023-11-02T17:01:26Z
url: https://github.com/astral-sh/ruff/issues/7067
synced_at: 2026-01-12T15:54:46Z
```

# Formatter collapses overlong assignment's parenthesized value

---

_@cnpryer_

Closest report I could find is #6271

Line-length: 88

```py
# Input
def f():
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa = (
        True
    )

# Black (23.7.0)
def f():
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa = (
        True
    )

# Ruff (0.0.287)
def f():
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa = True
```

[Black Playground](https://black.vercel.app/?version=main&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4ADcAFpdAD2IimZxl1N_WlbvK5V9Hvo0zZlhiNcotlgDB7bxaZbBIS7i9HHvnbCajGMUIFr--DNDeP0oafJeATSaNSH9GzI8pkS3XaKv-jPsai7bQVxaN824AdHr-MlyAAAAAICN8x74Dvl9AAF23QEAAACM5vfXscRn-wIAAAAABFla)

[Ruff Playground](https://play.ruff.rs/22307bf7-4d68-4163-9937-cb6a4609b6c1)

---

_Renamed from "Formatter un-expands overlong assignment target's parenthesized value" to "Formatter un-expands overlong assignment's parenthesized value" by @cnpryer on 2023-09-02 18:51_

---

_Label `formatter` added by @charliermarsh on 2023-09-02 18:56_

---

_Comment by @cnpryer on 2023-09-02 19:50_

Hmm. What's the best implementation here? 

Would `ExprConstant`'s `NeedsParentheses` need to be updated to consider if the expression breaks? The way `maybe_parenthesize` for `Expr` is called from `FormatStmtAssign` seems right to me.

https://github.com/astral-sh/ruff/blob/577280c8bea1e4bb1e1a37ca7e1325dc655b38bb/crates/ruff_python_formatter/src/expression/parentheses.rs#L68-L69

*Parenthesize if the expression breaks.* But IIUC since `ExprConstant`'s `NeedParentheses` will return `Never` then the `Parenthesize::IfBreaks` strategy will never be used.

Would it make more sense to add conditional logic to consider this when `needs_parentheses` is evaluated?

https://github.com/astral-sh/ruff/blob/577280c8bea1e4bb1e1a37ca7e1325dc655b38bb/crates/ruff_python_formatter/src/expression/mod.rs#L199-L206

In order to avoid reaching the `Never` match?

https://github.com/astral-sh/ruff/blob/577280c8bea1e4bb1e1a37ca7e1325dc655b38bb/crates/ruff_python_formatter/src/expression/mod.rs#L290-L299

---

_Comment by @cnpryer on 2023-09-03 19:08_

Using `OptionalParentheses::Multiline` would resolve this, but then the following would need to be addressed

```
Snapshot file: crates/ruff_python_formatter/tests/snapshots/format@expression__unsplittable.py.snap
Snapshot: unsplittable
Source: crates/ruff_python_formatter/tests/fixtures.rs:160
Input file: crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/unsplittable.py
────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  109   109 │ aaaaaaa = (
  110   110 │     1111111111111111111111111111111111111111111111111111111111111111111111111111111
  111   111 │ )
  112   112 │ 
  113       │-aaaaaaa = """111111111111111111111111111111111111111111111111111111111111111111111111111
        113 │+aaaaaaa = (
        114 │+    """111111111111111111111111111111111111111111111111111111111111111111111111111
  114   115 │ 1111111111111111111111111111111111111111111111111111111111111111111111111111111111111"""
        116 │+)
  115   117 │ 
  116   118 │ # Never parenthesize multiline strings
  117       │-aaaaaaa = """1111111111111111111111111111111111111111111111111111111111111111111111111111
        119 │+aaaaaaa = (
        120 │+    """1111111111111111111111111111111111111111111111111111111111111111111111111111
  118   121 │ 1111111111111111111111111111111111111111111111111111111111111111111111111111111111111"""
        122 │+)
```

---

_Comment by @MichaReiser on 2023-09-04 06:19_

This is related to https://github.com/astral-sh/ruff/pull/6816 and specifically this "optimisation"

https://github.com/astral-sh/ruff/blob/c05e4628b1d385d106e7e3d355f5d1d76fbfe401/crates/ruff_python_formatter/src/expression/parentheses.rs#L38-L50

My assumptions were:

* This pattern is rare
* Breaking doesn't improve readability significantly for very short right hand sides. 

Which is why I ultimately opted for better performance than using best fit for every constant/identifier (which are very common)

---

_Label `needs-decision` added by @MichaReiser on 2023-09-08 07:56_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-08 07:56_

---

_Label `needs-decision` removed by @MichaReiser on 2023-09-18 14:32_

---

_Closed by @MichaReiser on 2023-09-19 06:29_

---

_Renamed from "Formatter un-expands overlong assignment's parenthesized value" to "Formatter collapses overlong assignment's parenthesized value" by @cnpryer on 2023-11-02 17:01_

---
