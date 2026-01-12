```yaml
number: 10116
title: "Perf: Skip string normalization when possible"
type: pull_request
state: merged
author: MichaReiser
labels:
  - performance
  - formatter
assignees: []
merged: true
base: main
head: perf-string-normalization
created_at: 2024-02-25T10:08:36Z
updated_at: 2024-02-26T17:39:20Z
url: https://github.com/astral-sh/ruff/pull/10116
synced_at: 2026-01-12T15:55:31Z
```

# Perf: Skip string normalization when possible

---

_@MichaReiser_

## Summary

Most strings don't contain any quotes or other characters that need to be normalized. This PR makes use of this fact and short circuits `choose_quotes` and `normalize` for strings that contain no such characters. This is done by having one simple loop that searches for the occurrences of any character that needs normalization. This is much faster than the complicated normalization loop. 

This removes the calls to `StringNormalizer` almost entirely from the flamegraphs (except for users that use `\r` or `\r\n` newlines that always need normalisation ;( )


## Test Plan

`cargo test`


---

_Label `performance` added by @MichaReiser on 2024-02-25 10:10_

---

_Label `formatter` added by @MichaReiser on 2024-02-25 10:10_

---

_Comment by @codspeed-hq[bot] on 2024-02-25 10:16_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/perf-string-normalization)

### Merging #10116 will **improve performances by 18.79%**

<sub>Comparing <code>perf-string-normalization</code> (f7a3f92) with <code>main</code> (15b87ea)</sub>



### Summary

`⚡ 2` improvements
`✅ 28` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `perf-string-normalization` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `formatter[numpy/ctypeslib.py]` | 10.2 ms | 9.7 ms | +4.62% |
| ⚡ | `formatter[numpy/globals.py]` | 1,176.1 µs | 990.1 µs | +18.79% |


---

_Comment by @github-actions[bot] on 2024-02-25 10:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
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

_Marked ready for review by @MichaReiser on 2024-02-25 10:42_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-02-26 15:37_

---

_@charliermarsh reviewed on 2024-02-26 15:52_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/string/normalize.rs`:81 on 2024-02-26 15:52_

If it's not an f-string, you can omit `{`, I think? Similarly, if it's a raw string, you can omit `\\`... (In that case, you could actually use `memchr3`, but perhaps not worth it.)

---

_@charliermarsh approved on 2024-02-26 15:52_

---

_@MichaReiser reviewed on 2024-02-26 17:25_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/normalize.rs`:81 on 2024-02-26 17:25_

I considered special casing but decided against it because I want to avoid the extra complexity and it doesn't have to be perfect, for as long as it avoids the "expensive" normalization for most strings. 

 Using `memchr` would be nice... but `normalize` is already now almost gone from the benchmark profiles. That's why I consider this as "good enough".

---

_Merged by @MichaReiser on 2024-02-26 17:35_

---

_Closed by @MichaReiser on 2024-02-26 17:35_

---

_Branch deleted on 2024-02-26 17:35_

---
