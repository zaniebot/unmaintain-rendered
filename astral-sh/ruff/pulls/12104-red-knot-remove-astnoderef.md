```yaml
number: 12104
title: "[red-knot] Remove AstNodeRef"
type: pull_request
state: closed
author: MichaReiser
labels:
  - ty
assignees: []
base: main
head: remove-ast-node-ref
created_at: 2024-06-29T13:48:25Z
updated_at: 2024-08-12T07:52:46Z
url: https://github.com/astral-sh/ruff/pull/12104
synced_at: 2026-01-12T15:55:40Z
```

# [red-knot] Remove AstNodeRef

---

_@MichaReiser_

## Summary

The entire `AstNodeRef` hack isn't required anymore now that Salsa supports a `'db` lifetime. 
	
## Test Plan

`cargo test`


---

_Label `red-knot` added by @MichaReiser on 2024-06-29 13:48_

---

_Review requested from @carljm by @MichaReiser on 2024-06-29 13:48_

---

_Comment by @github-actions[bot] on 2024-06-29 14:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in `pyproject.toml`:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
  - 'unfixable' -> 'lint.unfixable'
  - 'per-file-ignores' -> 'lint.per-file-ignores'
warning: `PGH001` has been remapped to `S307`.
warning: `PGH002` has been remapped to `G010`.
warning: `PLR1701` has been remapped to `SIM101`.
ruff failed
  Cause: Selection of deprecated rule `E999` is not allowed when preview is enabled.
```

</p>
</details>

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/gpt_actions_library/.gpt_action_getting_started.ipynb:11:1:1: Expected an expression
error: Failed to parse examples/gpt_actions_library/gpt_action_bigquery.ipynb:13:1:1: Expected an expression
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 2 project errors)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>
<pre>ruff format --preview --exclude Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py</pre>
</p>
<p>

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in `pyproject.toml`:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
  - 'unfixable' -> 'lint.unfixable'
  - 'per-file-ignores' -> 'lint.per-file-ignores'
warning: `PGH001` has been remapped to `S307`.
warning: `PGH002` has been remapped to `G010`.
warning: `PLR1701` has been remapped to `SIM101`.
ruff failed
  Cause: Selection of deprecated rule `E999` is not allowed when preview is enabled.
```

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/gpt_actions_library/.gpt_action_getting_started.ipynb:11:1:1: Expected an expression
error: Failed to parse examples/gpt_actions_library/gpt_action_bigquery.ipynb:13:1:1: Expected an expression
```

</p>
</details>




---

_@charliermarsh approved on 2024-06-29 17:42_

---

_Comment by @MichaReiser on 2024-06-30 15:54_

I thought more about this and, while this seems "safe" due to Salsa lifetimes I think it isn't as soon as we add LRU caching or persistent caching. It could then happen that the pointers become stale if the AST gets evicted from the caches or if only restoring the symbol table from the persistent cache.

See https://salsa.zulipchat.com/#narrow/stream/333573-salsa-3.2E0/topic/proposed.20design/near/448130853

---

_Closed by @MichaReiser on 2024-06-30 15:54_

---

_Branch deleted on 2024-08-12 07:52_

---
