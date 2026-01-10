```yaml
number: 10119
title: "FString formatting: remove fstring handling in `normalize_string`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: normalize-fstring
created_at: 2024-02-25T12:06:49Z
updated_at: 2024-02-25T17:28:47Z
url: https://github.com/astral-sh/ruff/pull/10119
synced_at: 2026-01-10T22:57:10Z
```

# FString formatting: remove fstring handling in `normalize_string`

---

_Pull request opened by @MichaReiser on 2024-02-25 12:06_

## Summary

The fstring handling inside of `normalize_string` is no longer needed with the new fstring formatting. 
Gate it so that we don't forget to remove it when we promote f-string formating to stable.

## Test Plan

`cargo test`


---

_Review requested from @dhruvmanila by @MichaReiser on 2024-02-25 12:07_

---

_Label `internal` added by @MichaReiser on 2024-02-25 12:07_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/string/normalize.rs`:421 on 2024-02-25 12:13_

Shouldn't this be `format_fstring && ...` as per the variable name? Or an I misreading the condition?

---

_@dhruvmanila approved on 2024-02-25 12:13_

---

_Comment by @github-actions[bot] on 2024-02-25 12:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
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

_@MichaReiser reviewed on 2024-02-25 12:32_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/normalize.rs`:421 on 2024-02-25 12:32_

We could rename the variable. but the negation is intentional. We don't want to run the f-string code if `format_fstring` is enabled.

---

_@dhruvmanila reviewed on 2024-02-25 14:29_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/string/normalize.rs`:421 on 2024-02-25 14:29_

Oh I see. I think it was a mis-understanding from my side. This seems good 👍 

---

_Merged by @MichaReiser on 2024-02-25 17:28_

---

_Closed by @MichaReiser on 2024-02-25 17:28_

---

_Branch deleted on 2024-02-25 17:28_

---
