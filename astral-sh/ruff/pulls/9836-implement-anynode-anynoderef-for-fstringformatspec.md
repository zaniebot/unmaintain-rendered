```yaml
number: 9836
title: "Implement `AnyNode`/`AnyNodeRef` for `FStringFormatSpec`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/format-spec-anynode
created_at: 2024-02-05T16:15:39Z
updated_at: 2024-02-05T19:36:24Z
url: https://github.com/astral-sh/ruff/pull/9836
synced_at: 2026-01-12T15:55:30Z
```

# Implement `AnyNode`/`AnyNodeRef` for `FStringFormatSpec`

---

_@dhruvmanila_

## Summary

This PR adds the `AnyNode` and `AnyNodeRef` implementation for `FStringFormatSpec` node which will be required in the f-string formatting.

The main usage for this is so that we can pass in the node directly to `suppressed_node` in case debug expression is used to format is as verbatim text.

---

_Label `internal` added by @dhruvmanila on 2024-02-05 16:15_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-02-05 16:15_

---

_@MichaReiser approved on 2024-02-05 16:30_

---

_Merged by @dhruvmanila on 2024-02-05 19:23_

---

_Closed by @dhruvmanila on 2024-02-05 19:23_

---

_Branch deleted on 2024-02-05 19:23_

---

_Comment by @github-actions[bot] on 2024-02-05 19:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB113
	- FURB131
	- FURB132
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 2 project errors)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>
<pre>ruff format --no-preview --exclude tests/roots/test-pycode/cp_1251_coded.py</pre>
</p>
<p>

```
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB113
	- FURB131
	- FURB132
```

</p>
</details>
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
