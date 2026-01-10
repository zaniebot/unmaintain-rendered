```yaml
number: 12099
title: "Use `smol_str` for Identifier"
type: pull_request
state: closed
author: MichaReiser
labels:
  - performance
assignees: []
draft: true
base: main
head: smol_str
created_at: 2024-06-29T09:17:28Z
updated_at: 2024-08-12T07:52:53Z
url: https://github.com/astral-sh/ruff/pull/12099
synced_at: 2026-01-10T21:38:31Z
```

# Use `smol_str` for Identifier

---

_Pull request opened by @MichaReiser on 2024-06-29 09:17_

One more time. Trying to use `smol_str` for `Identifier`s to avoid heap allocations for strings shorter than 24 characters. 

I also expect this to help with `red_knot` where the symbol table can't store string slices for symbol names. Instead, the symbol table clones the identifier name which is `O(1)` for `smol_str`. 

Edit: It should be possible to store a `&Name` in the symbol table, now that salsa supports the `&'db` lifetime. That means, `O(1)` cloning is not as important anymore.

## Performance improvement

I think the performance improvement mainly comes from the removed `Box<str>` to `String` conversion in the hot `parse_name` function. It would be possible to remove that conversion by changing `ExprName` to store a `Box<str>`. 

For a comparison, https://github.com/astral-sh/ruff/pull/12100 uses a `Box<str>` instead of a `SmolStr`. The parser improvements are not as significant. But no linter benchmark regress.

General observation:

* Parsing becomes faster because of the removed cloning of the `ExprName` identifier
* I assume lexing becomes faster because it can bypass the heap allocation for most names
* Speedup in AST `drop`
* Some linter benchmarks become slower. I assume it is because dereferencing a `Name` now requires branching



---

_Comment by @codspeed-hq[bot] on 2024-06-29 09:23_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/smol_str)

### Merging #12099 will **improve performances by 6.89%**

<sub>Comparing <code>smol_str</code> (c157032) with <code>main</code> (d107968)</sub>



### Summary

`⚡ 1` improvements
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `smol_str` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 1.9 ms | 1.8 ms | +6.89% |


---

_Comment by @github-actions[bot] on 2024-06-29 09:46_

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

_Label `performance` added by @MichaReiser on 2024-06-29 10:16_

---

_Comment by @MichaReiser on 2024-06-29 10:17_

Closing in favor of https://github.com/astral-sh/ruff/pull/12101 which shows the most promising results.

---

_Closed by @MichaReiser on 2024-06-29 12:58_

---

_Reopened by @MichaReiser on 2024-06-29 19:12_

---

_Closed by @MichaReiser on 2024-06-29 19:19_

---

_Branch deleted on 2024-08-12 07:52_

---
