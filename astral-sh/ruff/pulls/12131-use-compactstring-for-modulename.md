```yaml
number: 12131
title: "Use `CompactString` for `ModuleName`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: module-name-compact-string
created_at: 2024-07-01T08:05:55Z
updated_at: 2024-07-01T08:29:12Z
url: https://github.com/astral-sh/ruff/pull/12131
synced_at: 2026-01-10T21:56:00Z
```

# Use `CompactString` for `ModuleName`

---

_Pull request opened by @MichaReiser on 2024-07-01 08:05_

## Summary

This PR refactors `ModuleName` to use `CompactString` instead of `smol_str` to reduce the number of small-string crates to one. 
`CompactString` showed significantely better performance in the parser and lexer benchmarks than `smol_str`.

One notable difference between the two crates is that `smol_str::clone` is `O(1)` whereas `CompactString` is more like a regular `String` with `O(n)` cloning. 
We'll see if the `O(n)` cloning becomes a problem in `red_knot` once we have proper benchmarks. For now, I'm going to assume it is okay. 

I refactored `red_knot` to use the `red_knot_module_resovler::ModuleName`. That code will be deleted soon anyway. 

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2024-07-01 08:05_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-07-01 08:05_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-07-01 08:05_

---

_Comment by @codspeed-hq[bot] on 2024-07-01 08:10_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/module-name-compact-string)

### Merging #12131 will **not alter performance**

<sub>Comparing <code>module-name-compact-string</code> (aa42b5d) with <code>main</code> (5109b50)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/module.rs`:64 on 2024-07-01 08:16_

I'd probably just write this function as

```rs
    fn is_valid_name(name: &str) -> bool {
        !name.is_empty() && name.split('.').all(is_identifier)
```

---

_@AlexWaygood approved on 2024-07-01 08:21_

Looks good! It'll cause some ~annoying merge conflicts for a change I'm working on locally, but that's okay

---

_Label `red-knot` added by @MichaReiser on 2024-07-01 08:21_

---

_Merged by @MichaReiser on 2024-07-01 08:22_

---

_Closed by @MichaReiser on 2024-07-01 08:22_

---

_Branch deleted on 2024-07-01 08:22_

---

_Comment by @github-actions[bot] on 2024-07-01 08:29_

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
