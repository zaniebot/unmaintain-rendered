```yaml
number: 12129
title: "Remove `demisto/content` from ecosystem checks"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/eco
created_at: 2024-07-01T02:14:49Z
updated_at: 2024-07-01T12:34:28Z
url: https://github.com/astral-sh/ruff/pull/12129
synced_at: 2026-01-12T15:55:40Z
```

# Remove `demisto/content` from ecosystem checks

---

_@charliermarsh_

## Summary

Unfortunately `demisto/content` uses an explicit `select` for `E999`, so it will _always_ fail in preview. And they're on a fairly old version. I'd like to keep checking it, but seems easiest for now to just disable it.

In response, I've added a few new repos.

---

_Comment by @github-actions[bot] on 2024-07-01 02:41_

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

_Review comment by @dhruvmanila on `scripts/check_ecosystem.py`:124 on 2024-07-01 03:40_

I think you meant to use `apache/superset` if I'm not mistaken.
```suggestion
    Repository("apache", "superset", "master", select="ALL"),
```

---

_@dhruvmanila approved on 2024-07-01 03:41_

---

_Comment by @dhruvmanila on 2024-07-01 03:42_

Huh, weird that it's not reflecting in the ecosystem comment on this PR.

Edit: I'm rerunning the job.

---

_Merged by @charliermarsh on 2024-07-01 12:20_

---

_Closed by @charliermarsh on 2024-07-01 12:20_

---

_Branch deleted on 2024-07-01 12:20_

---

_Comment by @codspeed-hq[bot] on 2024-07-01 12:21_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/eco)

### Merging #12129 will **improve performances by 7.57%**

<sub>Comparing <code>charlie/eco</code> (3153e49) with <code>charlie/eco</code> (0053c5a)</sub>



### Summary

`⚡ 6` improvements
`✅ 24` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `charlie/eco` | `charlie/eco` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `lexer[large/dataset.py]` | 1.1 ms | 1.1 ms | +6.06% |
| ⚡ | `lexer[numpy/ctypeslib.py]` | 227.2 µs | 216.3 µs | +5.06% |
| ⚡ | `lexer[pydantic/types.py]` | 506.1 µs | 481.6 µs | +5.11% |
| ⚡ | `lexer[unicode/pypinyin.py]` | 77.4 µs | 74.2 µs | +4.34% |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 1.9 ms | 1.8 ms | +7.57% |
| ⚡ | `parser[numpy/ctypeslib.py]` | 953.7 µs | 915 µs | +4.23% |


---
