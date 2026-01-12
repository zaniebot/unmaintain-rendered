```yaml
number: 9674
title: "Set source type: Stub for black tests with options"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: set-source-type-for-stub-files-in-black-tests
created_at: 2024-01-29T14:43:32Z
updated_at: 2024-01-29T14:54:31Z
url: https://github.com/astral-sh/ruff/pull/9674
synced_at: 2026-01-12T15:55:29Z
```

# Set source type: Stub for black tests with options

---

_@MichaReiser_

## Summary

Set the `source_type` for black tests with options to `Stub` to ensure they run as stubs and not normal Python files.

## Test Plan

`nested_stub.pyi` now shows a diff


---

_Label `internal` added by @MichaReiser on 2024-01-29 14:43_

---

_Label `formatter` added by @MichaReiser on 2024-01-29 14:43_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-01-29 14:44_

---

_Comment by @github-actions[bot] on 2024-01-29 14:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/dalle/Image_generations_edits_and_variations_with_DALL-E.ipynb:3:7:8: Unexpected token 'prompt'
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
error: Failed to parse examples/dalle/Image_generations_edits_and_variations_with_DALL-E.ipynb:3:7:8: Unexpected token 'prompt'
```

</p>
</details>




---

_Merged by @MichaReiser on 2024-01-29 14:54_

---

_Closed by @MichaReiser on 2024-01-29 14:54_

---

_Branch deleted on 2024-01-29 14:54_

---
