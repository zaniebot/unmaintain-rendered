```yaml
number: 15471
title: Fix curly bracket spacing around curly f-string expressions
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: micha/formatter-fix-f-string-curly-spacing
created_at: 2025-01-14T10:16:42Z
updated_at: 2025-01-15T08:22:50Z
url: https://github.com/astral-sh/ruff/pull/15471
synced_at: 2026-01-10T20:34:00Z
```

# Fix curly bracket spacing around curly f-string expressions

---

_Pull request opened by @MichaReiser on 2025-01-14 10:16_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/15459

F-string expression need to leave one space between the element's `{` or `}` and any curly braces inside the expression itself or it results in invalid syntax:

```python
f"{ {1, 2, 3} }"  # valid
f"{{ 1, 2, 3}}"  # invalid
```

The existing implementation handles this correctly if the expression itself has curly braces, but it doesn't add the necessary space if the expression starts or ends with an expression with curly parentheses:

```python
-print(f"{ {1, 2, 3} - {2} }")
-print(f"{ {1: 2}.keys() }")
+print(f"{{1, 2, 3} - {2}}")
+print(f"{{1: 2}.keys()}")
```

This PR changes the formatter to check if the left-most sub expression (if any) starts with curly parentheses instead of just checking the expression itself.

## Test Plan

Added snapshot tests.


---

_Label `bug` added by @MichaReiser on 2025-01-14 10:16_

---

_Label `formatter` added by @MichaReiser on 2025-01-14 10:16_

---

_Comment by @github-actions[bot] on 2025-01-14 10:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/Assistants_API_overview_python.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 197 column 8
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
error: Failed to read examples/Assistants_API_overview_python.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 197 column 8
```

</p>
</details>




---

_Marked ready for review by @MichaReiser on 2025-01-14 10:23_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-01-14 10:23_

---

_Review requested from @charliermarsh by @MichaReiser on 2025-01-14 16:47_

---

_@dhruvmanila reviewed on 2025-01-15 06:02_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/other/f_string_element.rs`:214 on 2025-01-15 06:02_

I wonder if we should just check whether the leftmost expression has curly braces otherwise the formatter will add spaces to the following even though it's not required:

```py
f"{foo or {}}"

# From the test case
print(f"{1, 2, {3}}")
```

I think I'd consider the argument about maintaining consistency on both sides to be valid only when one of the side needs to add the curly brace.

Currently, the above snippet will be formatted as:
```py
f"{ foo or {} }"

# From the test case
print(f" {1, 2, {3} }")
```

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-01-15 08:11_

---

_@dhruvmanila approved on 2025-01-15 08:20_

Thank you!

---

_Merged by @MichaReiser on 2025-01-15 08:22_

---

_Closed by @MichaReiser on 2025-01-15 08:22_

---

_Branch deleted on 2025-01-15 08:22_

---
