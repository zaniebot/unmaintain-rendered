```yaml
number: 15537
title: Fix bracket spacing for single-element tuples in f-string expressions
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: micha/f-string-tuple
created_at: 2025-01-16T17:51:58Z
updated_at: 2025-01-17T08:03:55Z
url: https://github.com/astral-sh/ruff/pull/15537
synced_at: 2026-01-10T20:34:00Z
```

# Fix bracket spacing for single-element tuples in f-string expressions

---

_Pull request opened by @MichaReiser on 2025-01-16 17:51_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/15535#issuecomment-2596286050

The formatter always parenthesizes single-element tuples even if they weren't in the source. 
This lead to an instability before where the formatter added spacing around the f-strings `{ {}, }` that
then were removed when reformatting the code because the tuple's now parenthesized `{({},)}`. 

## Test Plan

Added test. I reviewed our other expression formatting and I didn't find any
other, except generators, that introduce parentheses. For generators, unparenthesized
generators aren't allowed in an expression context. 


---

_Label `bug` added by @MichaReiser on 2025-01-16 17:52_

---

_Label `formatter` added by @MichaReiser on 2025-01-16 17:52_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-01-16 17:52_

---

_Comment by @github-actions[bot] on 2025-01-16 18:04_

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

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/other/f_string_element.rs`:213 on 2025-01-17 05:26_

Should we use `.then` here to avoid eager evaluation? Regardless, I don't think we need to wrap it in `Some(..)`.

```rs
            let bracket_spacing = needs_bracket_spacing(expression, f.context()).then(|| {
                format_with(|f| {
                    if self.context.can_contain_line_breaks() {
                        soft_line_break_or_space().fmt(f)
                    } else {
                        space().fmt(f)
                    }
                })
            });
```

---

_@dhruvmanila approved on 2025-01-17 05:26_

Thank you!

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_string_element.rs`:213 on 2025-01-17 07:55_

Oh yeah, the `Some` isn't necessary. I think `then_some` is fine because `format_with` is very cheap, it roughly boils down to returning a function pointer that then gets assigned to `bracket_spacing`. Calling a function is probably more expensive

---

_@MichaReiser reviewed on 2025-01-17 07:55_

---

_Merged by @MichaReiser on 2025-01-17 08:02_

---

_Closed by @MichaReiser on 2025-01-17 08:02_

---

_Branch deleted on 2025-01-17 08:02_

---
