```yaml
number: 9699
title: Show source-type in formatter snapshot tests with options
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: test-snapshots-source-type
created_at: 2024-01-30T10:03:06Z
updated_at: 2024-01-30T10:13:16Z
url: https://github.com/astral-sh/ruff/pull/9699
synced_at: 2026-01-10T22:57:09Z
```

# Show source-type in formatter snapshot tests with options

---

_Pull request opened by @MichaReiser on 2024-01-30 10:03_

## Summary

Show the source type in formatter snapshot tests with an `.options.json` file to ease debugging.

## Test Plan

Reviewed snapshots


---

_Label `internal` added by @MichaReiser on 2024-01-30 10:03_

---

_Label `formatter` added by @MichaReiser on 2024-01-30 10:03_

---

_Merged by @MichaReiser on 2024-01-30 10:08_

---

_Closed by @MichaReiser on 2024-01-30 10:08_

---

_Branch deleted on 2024-01-30 10:08_

---

_Comment by @github-actions[bot] on 2024-01-30 10:13_

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
