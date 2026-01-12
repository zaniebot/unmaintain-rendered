```yaml
number: 19183
title: "Fix `unreachable` panic in parser"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - parser
assignees: []
merged: true
base: main
head: parse-bug
created_at: 2025-07-07T14:54:36Z
updated_at: 2025-07-20T22:12:32Z
url: https://github.com/astral-sh/ruff/pull/19183
synced_at: 2026-01-12T15:56:33Z
```

# Fix `unreachable` panic in parser

---

_@dylwil3_

Parsing the (invalid) expression `f"{\t"i}"` caused a panic because the `TStringMiddle` character was "unreachable" due the way the parser recovered from the line continuation (it ate the t-string start).

The cause of the issue is as follows: 

The parser begins parsing the f-string and expects to see a list of objects, essentially alternating between _interpolated elements_ and ordinary strings. It is happy to see the first left brace, but then there is a lexical error caused by the line-continuation character. So instead of the parser seeing a list of elements with just one member, it sees a list that starts like this:

- Interpolated element with an invalid token, stored as a `Name`
- Something else built from tokens beginning with `TStringStart` and `TStringMiddle`

When it sees the `TStringStart` error recovery says "that's a list element I don't know what to do with, let's skip it". When it sees `TStringMiddle` it says "oh, that looks like the middle of _some interpolated string_ so let's try to parse it as one of the literal elements of my `FString`". Unfortunately, the function being used to parse individual list elements thinks (arguably correctly) that it's not possible to have a `TStringMiddle` sitting in your `FString`, and hits `unreachable`.

Two potential ways (among many) to solve this issue are:

1. Allow a `TStringMiddle` as a valid "literal" part of an f-string during parsing (with the hope/understanding that this would only occur in an invalid context)
2. Skip the `TStringMiddle` as an "unexpected/invalid list item" in the same way that we skipped `TStringStart`.

I have opted for the second approach since it seems somehow more morally correct, even though it loses more information. To implement this, the recovery context needs to know whether we are in an f-string or t-string - hence the changes to that enum. As a bonus we get slightly more specific error messages in some cases.

Closes #18860


---

_Label `bug` added by @dylwil3 on 2025-07-07 14:54_

---

_Label `parser` added by @dylwil3 on 2025-07-07 14:54_

---

_@dylwil3 reviewed on 2025-07-07 14:55_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/parser/expression.rs`:1694 on 2025-07-07 14:55_

I'm unsure about this part. If I _don't_ do this, then the parser gets stuck and fails an `assert_progressing` call. On the other hand it seems weird that I have to essentially bump _twice_, if I'm following the logic correctly?

---

_Comment by @github-actions[bot] on 2025-07-07 15:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/mcp/databricks_mcp_cookbook.ipynb:12:1:8: Simple statements must be separated by newlines or semicolons
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/mcp/databricks_mcp_cookbook.ipynb:12:1:8: Simple statements must be separated by newlines or semicolons
```

</p>
</details>




---

_@MichaReiser reviewed on 2025-07-07 15:51_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1694 on 2025-07-07 15:51_

What are the tokens that you bump with `bump_any`. What's the token that gets bumped with `add_error`?

---

_Comment by @dylwil3 on 2025-07-18 21:51_

@MichaReiser okay after a more thorough investigation I have a better understanding of what is going on here and have gone with a different fix. I've tried to summarize my reasoning in the updated PR summary. Let me know whether you agree, or whether you'd like me to solve the issue in a different way.

---

_Marked ready for review by @dylwil3 on 2025-07-18 21:52_

---

_Review requested from @dhruvmanila by @dylwil3 on 2025-07-18 21:52_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/mod.rs`:1189 on 2025-07-19 13:54_

Do we have a similar problem with the `FormatSpec` that it accepts both t-string and f-strings. 



---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/mod.rs`:1191 on 2025-07-19 13:54_

I think the indentation here is off

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/mod.rs`:811 on 2025-07-19 13:56_

Not really related to your PR, but all those types could implement `Eq`

---

_@MichaReiser approved on 2025-07-19 13:56_

Makes sense. Thank you

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/parser/mod.rs`:1189 on 2025-07-20 21:58_

I don't think so because the `FormatSpec` doesn't contain an analog of `T/FStringMiddle`

---

_@dylwil3 reviewed on 2025-07-20 21:58_

---

_Merged by @dylwil3 on 2025-07-20 22:04_

---

_Closed by @dylwil3 on 2025-07-20 22:04_

---
