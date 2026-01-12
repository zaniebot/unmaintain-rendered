```yaml
number: 15524
title: "Fix joining of f-strings with different quotes when using quote style `Preserve`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: micha/fix-implicit-concat-preserve
created_at: 2025-01-16T07:42:45Z
updated_at: 2025-01-16T11:02:55Z
url: https://github.com/astral-sh/ruff/pull/15524
synced_at: 2026-01-12T15:55:51Z
```

# Fix joining of f-strings with different quotes when using quote style `Preserve`

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/15514

Fixes joining implicit concatenated strings when using `quote-style="Preserve` if the parts use different quotes and at least one contains a nested string using quotes. 

The bug rooted from always returning `Preserve` in the `choose_quotes` function when using `quote-style="Preserve"` even in cases where f-strings are nested. 
This is fine for regular f-strings (because we can assume that the quotes were valid in the source) but it isn't sufficient when joining f-strings. 

## Test Plan

Added tests


---

_Review requested from @dhruvmanila by @MichaReiser on 2025-01-16 07:42_

---

_Comment by @MichaReiser on 2025-01-16 07:43_

I've to run now but I want to spend some more time on adding more tests. @dhruvmanila maybe you have some ideas of other edge cases worth testing. But I think this is otherwise ready to go.

---

_Label `bug` added by @MichaReiser on 2025-01-16 07:43_

---

_Label `formatter` added by @MichaReiser on 2025-01-16 07:43_

---

_Comment by @github-actions[bot] on 2025-01-16 07:51_

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

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/join_implicit_concatenated_string_preserve.py`:22 on 2025-01-16 09:43_

We might want to add some test cases which includes format spec.

Is the following correct?
```diff
 params = {}
-string = "this is my string with " f'"{params.get("mine")=:{"xxx"}}"'
+string = f'this is my string with "{params.get("mine")=:{"xxx"}}"'
```

with quote-style `preserve` and < 3.12 Python.

Or, is there no way to preserve the quote style here?

---

_@dhruvmanila reviewed on 2025-01-16 09:43_

---

_@dhruvmanila reviewed on 2025-01-16 09:44_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/join_implicit_concatenated_string_preserve.py`:22 on 2025-01-16 09:44_

This doesn't seem to be relevant to this PR though.

---

_@dhruvmanila approved on 2025-01-16 09:44_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/join_implicit_concatenated_string_preserve.py`:22 on 2025-01-16 11:00_

There's no strictly correct answer when it comes to preserve and joining implicit concatenated strings if not all parts use the same quotes. You have to choose which quotes you want to preserve and in this example, the formatter decides to preserve the quotes from the second part because the string can't be joined otherwise.

---

_@MichaReiser reviewed on 2025-01-16 11:00_

---

_Merged by @MichaReiser on 2025-01-16 11:01_

---

_Closed by @MichaReiser on 2025-01-16 11:01_

---

_Branch deleted on 2025-01-16 11:01_

---

_@dhruvmanila reviewed on 2025-01-16 11:02_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/join_implicit_concatenated_string_preserve.py`:22 on 2025-01-16 11:02_

That sounds reasonable, thanks

---
