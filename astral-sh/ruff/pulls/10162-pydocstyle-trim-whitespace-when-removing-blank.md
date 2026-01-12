```yaml
number: 10162
title: "[`pydocstyle`] Trim whitespace when removing blank lines after section (`D413`)"
type: pull_request
state: merged
author: jusexton
labels:
  - fixes
assignees: []
merged: true
base: main
head: fix-d413-issue
created_at: 2024-02-29T07:12:46Z
updated_at: 2024-02-29T13:41:31Z
url: https://github.com/astral-sh/ruff/pull/10162
synced_at: 2026-01-12T15:55:31Z
```

# [`pydocstyle`] Trim whitespace when removing blank lines after section (`D413`)

---

_@jusexton_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/10050

## Test Plan

Added additional scenario to `D413.py` and updated snapshots.


---

_Renamed from "Fixed bug applying whitespace when applying fix for D413." to "Fixed bug creating unecessary whitespace when applying fix for D413." by @jusexton on 2024-02-29 07:27_

---

_Comment by @github-actions[bot] on 2024-02-29 07:30_

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
error: Failed to read examples/How_to_handle_rate_limits.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: trailing comma at line 47 column 4
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
error: Failed to read examples/How_to_handle_rate_limits.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: trailing comma at line 47 column 4
```

</p>
</details>




---

_Label `fixes` added by @MichaReiser on 2024-02-29 13:09_

---

_Renamed from "Fixed bug creating unecessary whitespace when applying fix for D413." to "[`pydocstyle`] Trim whitespace when removing blank lines after section (`D413`)" by @MichaReiser on 2024-02-29 13:20_

---

_Comment by @MichaReiser on 2024-02-29 13:20_

Thank you

---

_Merged by @MichaReiser on 2024-02-29 13:29_

---

_Closed by @MichaReiser on 2024-02-29 13:29_

---
