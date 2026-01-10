```yaml
number: 15545
title: Fix unstable f-string formatting for expressions containing a trailing comma
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: micha/f-string-trailing-comma
created_at: 2025-01-17T07:52:48Z
updated_at: 2025-01-17T09:08:12Z
url: https://github.com/astral-sh/ruff/pull/15545
synced_at: 2026-01-10T20:34:00Z
```

# Fix unstable f-string formatting for expressions containing a trailing comma

---

_Pull request opened by @MichaReiser on 2025-01-17 07:52_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/15536

The `RemoveSoftlinesBuffer` removes soft line breaks and `if_group_breaks` elements but it
doesn't remove `expand_parent` elements. This can lead to instable formatting
when f-strings are wrapped in a `group` because it makes that group expand. For example, 
`print(f"{ {}, 1, }")` first gets formatted to 

```py
print(
    f"{ {}, 1 }"
)
```

and only then converges at `print(f"{ {}, 1 }")`. 

I first tried to also remove `expand_parent` elemnents in the `RemoveSoftLinesBuffer`
but that results in many failing tests around assignment formatting because 
we rely on `Document::will_break` to determine if we should try the flat 
layout and that only returns `true` if the f-string formatting contains the `expand_parent` along the trailing comma.

I reviewed all `expand_parent` usages and realized that the trailing comma is the only 
usage in our expression formatting. That's why I decided to adjust the trailing comma
formatting instead to omit the trailing comma if we know that we're in a flat f-string.

This is also how Dhruv implemented it originally before I removed the check in https://github.com/astral-sh/ruff/pull/14489 because I thought it
it no longer necessary. 

## Test Plan

Added regression test


---

_Label `bug` added by @MichaReiser on 2025-01-17 07:53_

---

_Label `formatter` added by @MichaReiser on 2025-01-17 07:53_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-01-17 07:53_

---

_Marked ready for review by @MichaReiser on 2025-01-17 07:53_

---

_Comment by @github-actions[bot] on 2025-01-17 08:07_

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

_@dhruvmanila approved on 2025-01-17 09:06_

---

_Renamed from "Fix instable f-string formatting for expressions containing a trailing comma" to "Fix unstable f-string formatting for expressions containing a trailing comma" by @dhruvmanila on 2025-01-17 09:06_

---

_Merged by @MichaReiser on 2025-01-17 09:08_

---

_Closed by @MichaReiser on 2025-01-17 09:08_

---

_Branch deleted on 2025-01-17 09:08_

---
