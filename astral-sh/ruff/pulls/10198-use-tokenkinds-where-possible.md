```yaml
number: 10198
title: Use TokenKinds where possible
type: pull_request
state: closed
author: MichaReiser
labels: []
assignees: []
draft: true
base: main
head: token-kinds
created_at: 2024-03-02T16:01:33Z
updated_at: 2024-08-12T07:53:28Z
url: https://github.com/astral-sh/ruff/pull/10198
synced_at: 2026-01-12T15:55:31Z
```

# Use TokenKinds where possible

---

_@MichaReiser_

## Summary

I don't expect this PR to improve performance because cosntructing the `SpannedKind`s vector is probably too expensive. 

The idea is to use a `&[(TokenKind, TextRange)]` for rules that don't need the full tokens. 
	
## Test Plan

`cargo test`


---

_Comment by @codspeed-hq[bot] on 2024-03-02 16:06_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/token-kinds)

### Merging #10198 will **not alter performance**

<sub>Comparing <code>token-kinds</code> (98cd3e7) with <code>main</code> (c007b17)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-03-02 16:38_

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

_Closed by @MichaReiser on 2024-03-02 17:54_

---

_Branch deleted on 2024-08-12 07:53_

---
