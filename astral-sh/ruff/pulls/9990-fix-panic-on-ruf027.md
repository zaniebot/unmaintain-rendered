```yaml
number: 9990
title: Fix panic on RUF027
type: pull_request
state: merged
author: snowsignal
labels:
  - bug
assignees: []
merged: true
base: main
head: jane/ruf027/slice-panic-fix
created_at: 2024-02-14T20:11:46Z
updated_at: 2024-02-16T20:10:05Z
url: https://github.com/astral-sh/ruff/pull/9990
synced_at: 2026-01-10T22:57:09Z
```

# Fix panic on RUF027

---

_Pull request opened by @snowsignal on 2024-02-14 20:11_

## Summary

Fixes #9895 

The cause for this panic came from an offset error in the code. When analyzing a hypothetical f-string, we attempt to re-parse it as an f-string, and use the AST data to determine, among other things, whether the format specifiers are correct. To determine the 'correctness' of a format specifier, we actually have to re-parse the format specifier, and this is where the issue lies. To get the source text for the specifier, we were taking a slice from the original file source text... even though the AST data for the specifier belongs to the standalone parsed f-string expression, meaning that the ranges are going to be way off. In a file with Unicode, this can cause panics if the slice is inside a char boundary.

To fix this, we now slice from the temporary source we created earlier to parse the literal as an f-string.

## Test Plan

The RUF027 snapshot test was amended to include a string with format specifiers which we _should_ be calling out. This is to ensure we do slice format specifiers from the source text correctly.

---

_Label `bug` added by @snowsignal on 2024-02-14 20:11_

---

_Review requested from @MichaReiser by @snowsignal on 2024-02-14 20:13_

---

_Comment by @github-actions[bot] on 2024-02-14 20:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/aa6ab37499268e01e21610e6bb201c9659c02fb3/pandas/io/formats/format.py#L1919'>pandas/io/formats/format.py:1919:26:</a> RUF027 Possible f-string without an `f` prefix
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF027 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:94 on 2024-02-14 22:48_

I believe there's a parse_expression_at method in the parser that gives you absolute offsets (less confusing)

---

_@MichaReiser reviewed on 2024-02-14 22:49_

---

_Review comment by @snowsignal on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:94 on 2024-02-14 22:55_

Oh great! That would definitely make things simpler 😁 

---

_@snowsignal reviewed on 2024-02-14 22:55_

---

_@snowsignal reviewed on 2024-02-14 23:03_

---

_Review comment by @snowsignal on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:94 on 2024-02-14 23:03_

Actually, wait - I think it's a bit more complicated since putting the `f` at the front changes the resulting offset. We could just add 1 to the start offset (that was my original approach, actually) but I think slicing from the parsed string would be simpler and more immediately understandable.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/ruff/RUF027_0.py`:74 on 2024-02-15 04:33_

Does this test case panic on main? It's not panicking for me. I think we can use the same code as mentioned in the issue.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:91 on 2024-02-15 04:33_

nit: we probably don't require the call to `range` because `locator.slice` expects a `Ranged` type

```suggestion
    let fstring_expr = format!("f{}", locator.slice(literal)));
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:94 on 2024-02-15 04:35_

We've used code like `"f".text_len()` in multiple places but I guess this is fine as well.

---

_@dhruvmanila approved on 2024-02-15 04:36_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:94 on 2024-02-15 09:57_

Ah good point. I missed the fact that we parse a modified string. We could subtract `-1` from the start but that would fail apart if the string is at the very start of the file (not unlikely for docstrings). 

I think it's fine. It might be worth adding a comment somewhere mentioning that all offsets are relative to `fstring_expr`

---

_@MichaReiser approved on 2024-02-15 09:58_

---

_@snowsignal reviewed on 2024-02-16 18:41_

---

_Review comment by @snowsignal on `crates/ruff_linter/resources/test/fixtures/ruff/RUF027_0.py`:74 on 2024-02-16 18:41_

Oh yeah, this wasn't meant to test for a panic, this was to test that we _do_ parse format specifiers correctly.

I'll add the code from the original issue as an additional snapshot test :)

---

_Merged by @snowsignal on 2024-02-16 20:04_

---

_Closed by @snowsignal on 2024-02-16 20:04_

---

_Branch deleted on 2024-02-16 20:04_

---
