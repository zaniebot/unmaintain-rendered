```yaml
number: 12103
title: "Remove allocation in `parse_identifier`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - performance
  - parser
assignees: []
merged: true
base: main
head: parse-identifier-reduce-alloc
created_at: 2024-06-29T12:55:17Z
updated_at: 2024-06-29T19:38:57Z
url: https://github.com/astral-sh/ruff/pull/12103
synced_at: 2026-01-12T15:55:40Z
```

# Remove allocation in `parse_identifier`

---

_@MichaReiser_

## Summary

I think this PR removes an allocation from `parse_identifier`. 

`parse_identifier` converts the `Box<str>` from the `TokenValue::Name` to a `String` by calling `.to_string`. 
`to_string` is a small wrapper around `format!("{name}")` which allocates a new string without reusing the `Box<str>` allocation, unless the compiler can see through the operation. 

This PR uses `Box<str>::into_string` which, to my knowledge, reuses the underlying allocation. 

## Test Plan

`cargo test`


---

_Review requested from @dhruvmanila by @MichaReiser on 2024-06-29 12:55_

---

_Label `performance` added by @MichaReiser on 2024-06-29 12:55_

---

_Label `parser` added by @MichaReiser on 2024-06-29 12:55_

---

_Comment by @codspeed-hq[bot] on 2024-06-29 12:59_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/parse-identifier-reduce-alloc)

### Merging #12103 will **improve performances by 7.44%**

<sub>Comparing <code>parse-identifier-reduce-alloc</code> (83894c9) with <code>main</code> (47b2273)</sub>



### Summary

`⚡ 4` improvements
`✅ 26` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `parse-identifier-reduce-alloc` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[large/dataset.py]` | 5.4 ms | 5 ms | +7.44% |
| ⚡ | `parser[numpy/ctypeslib.py]` | 1,007.9 µs | 954.6 µs | +5.59% |
| ⚡ | `parser[pydantic/types.py]` | 2.2 ms | 2.1 ms | +5.9% |
| ⚡ | `parser[unicode/pypinyin.py]` | 329.3 µs | 315 µs | +4.54% |


---

_Comment by @MichaReiser on 2024-06-29 13:00_

I'll go ahead and merge this. This change should be pretty uncontroversial :laughing: 

---

_Merged by @MichaReiser on 2024-06-29 13:00_

---

_Closed by @MichaReiser on 2024-06-29 13:00_

---

_Branch deleted on 2024-06-29 13:00_

---

_Comment by @github-actions[bot] on 2024-06-29 13:13_

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

_Renamed from "Remove allcation in `parse_identifier`" to "Remove allocation in `parse_identifier`" by @AlexWaygood on 2024-06-29 19:38_

---
