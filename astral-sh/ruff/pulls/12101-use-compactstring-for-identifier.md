```yaml
number: 12101
title: "Use `CompactString` for `Identifier`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - performance
assignees: []
merged: true
base: main
head: identifier-compact_str
created_at: 2024-06-29T09:55:02Z
updated_at: 2024-07-01T08:07:02Z
url: https://github.com/astral-sh/ruff/pull/12101
synced_at: 2026-01-12T15:55:40Z
```

# Use `CompactString` for `Identifier`

---

_@MichaReiser_

This PR introduces a small string create for `ExprName` and `Identifier` to reduce the number of allocations. 


* moves the `Name` struct from `red_knot_python_semantic` to `ruff_python_ast`
* Change `ExprName` and `Identifier` to store a `Name` instead of a `String`
* Change `Name` to use `CompactString` which shows better performance than `smol_str`



## Performance improvement

* Less time spent allocating
* Less time spent deallocating (drop). This especially evident in the linter benchmarks

This change should also reduce peak-memory usage.

## Why `CompactString`

`CompactString` shows better performance in the read path than `smol_str`. The only disadvantage compared to `smol_str` is that `smol_str` supports `O(1)` cloning. ~~I had to update the red-knot symbol table to store references to avoid allocating new strings (it actually already allocated new strings, but we could have removed those allocations when the AST stores `smol_str`)~~. 




---

_Renamed from "Use `CompactString` for `Identifie`" to "Use `CompactString` for `Identifier`" by @MichaReiser on 2024-06-29 09:56_

---

_Comment by @codspeed-hq[bot] on 2024-06-29 09:59_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/identifier-compact_str)

### Merging #12101 will **improve performances by 7.54%**

<sub>Comparing <code>identifier-compact_str</code> (43ea86c) with <code>main</code> (db6ee74)</sub>



### Summary

`⚡ 6` improvements
`✅ 24` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `identifier-compact_str` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `lexer[large/dataset.py]` | 1.1 ms | 1.1 ms | +6.06% |
| ⚡ | `lexer[numpy/ctypeslib.py]` | 227.2 µs | 216.3 µs | +5.06% |
| ⚡ | `lexer[pydantic/types.py]` | 506.2 µs | 481.6 µs | +5.11% |
| ⚡ | `lexer[unicode/pypinyin.py]` | 77.4 µs | 74.2 µs | +4.3% |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 1.9 ms | 1.8 ms | +7.54% |
| ⚡ | `parser[numpy/ctypeslib.py]` | 953.7 µs | 915 µs | +4.23% |


---

_Comment by @github-actions[bot] on 2024-06-29 10:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 1 project error; 48 projects unchanged)

<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python/typeshed/blob/6e333e6ba736b84ffca5b977920757728e6bd86f/stdlib/_collections_abc.pyi#L10'>stdlib/_collections_abc.pyi:10:5:</a> PYI057 Do not use `typing.ByteString`, which has unclear semantics and is deprecated
</pre>

</p>
</details>
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
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI057 | 1 | 0 | 1 | 0 | 0 |

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

_Label `performance` added by @MichaReiser on 2024-06-29 10:17_

---

_Marked ready for review by @MichaReiser on 2024-06-29 13:40_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-06-29 13:40_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-06-29 13:40_

---

_Comment by @charliermarsh on 2024-06-29 16:41_

Nice!

---

_@charliermarsh reviewed on 2024-06-29 17:46_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/name.rs`:28 on 2024-06-29 17:46_

Nit: can you add your name to the TODO? (Also: typo in `ComactString`)

---

_@charliermarsh approved on 2024-06-29 17:48_

Nice. I'm a fan of this, and it's a great sign that we can now do it and see such a significant, measurable impact.

---

_Comment by @charliermarsh on 2024-06-29 17:49_

How did you determine that `CompactString` shows better results than `smol_str`? (It seems like a totally reasonable conclusion to me, just curious.)

---

_Comment by @MichaReiser on 2024-06-29 19:16_

Sorry, I should have mentioned this in the PR description. 

I first started by using `smol_str` because the `O(1)` cloning is nice, see https://github.com/astral-sh/ruff/pull/12099. The PR looked good at first because it significantly improved performance. But that was too good to be true and I realised that it mainly was because of an [unnecessary allocation in `parse_identifier`](https://github.com/astral-sh/ruff/pull/12103).  I also noticed that the lexer and parser benchmarks improved across the board, but that many linter benchmarks regressed, probably because accessing a string now required more branching. That's when I started to try out `CompactString` which showed better improvements in the Lexer and Parser benchmarks, without regressing the Linter benchmarks as much.


I rebased and reopened https://github.com/astral-sh/ruff/pull/12099 for a direct comparison.

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/name.rs`:11 on 2024-07-01 04:06_

nit: trailing comma
```suggestion
    derive(serde::Serialize, serde::Deserialize, ruff_macros::CacheKey)
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/refurb/rules/reimplemented_starmap.rs`:241 on 2024-07-01 04:11_

Do you think it would be useful to use `Into<Name>` in the functions like this? That way one can directly pass in `String` / `&str` and not worry about constructing the `Name`. If it's too much work (I guess it is?), it's fine not to do it.

---

_@dhruvmanila approved on 2024-07-01 04:14_

This is great! Thanks for doing this.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/name.rs`:28 on 2024-07-01 07:28_

Sure, what's the benefit of doing it? I'm not necessarily the person fixing it and the git history already shows who added the TODO.

---

_@MichaReiser reviewed on 2024-07-01 07:28_

---

_@MichaReiser reviewed on 2024-07-01 07:31_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/reimplemented_starmap.rs`:241 on 2024-07-01 07:31_

Not sure. I'm somewhat concerned about accidental monomorphization because some of the methods aren't trivial. 

---

_Review requested from @carljm by @MichaReiser on 2024-07-01 07:48_

---

_@dhruvmanila reviewed on 2024-07-01 07:58_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/refurb/rules/reimplemented_starmap.rs`:241 on 2024-07-01 07:58_

It's fine to not do it, just wondered if it would be useful.

---

_Merged by @MichaReiser on 2024-07-01 08:06_

---

_Closed by @MichaReiser on 2024-07-01 08:06_

---

_Branch deleted on 2024-07-01 08:06_

---
