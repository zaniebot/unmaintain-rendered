```yaml
number: 12218
title: "[red-knot] Exclude drop time in benchmark"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: red-knot-bench-exclude-drop
created_at: 2024-07-06T15:19:32Z
updated_at: 2024-07-06T15:36:17Z
url: https://github.com/astral-sh/ruff/pull/12218
synced_at: 2026-01-10T21:47:02Z
```

# [red-knot] Exclude drop time in benchmark

---

_Pull request opened by @MichaReiser on 2024-07-06 15:19_

## Summary

This PR changes the red knot benchmarks to exclude the `Program`'s `drop` time. 

The primary goal of the benchmarks is to measure the time it takes to check a single file. They do not measure a database's setup and teardown cost, which is also why the `Program` is passed into the benchmark function and not constructed inside of it. 

This doesn't mean that slow drop times aren't a concern—they are. But there are ways to mitigate them. For example, it's fine for the CLI to use `std::mem::forget` to leak memory before exiting because the OS will collect all memory anyway.


## Test Plan

See CI


---

_Label `red-knot` added by @MichaReiser on 2024-07-06 15:23_

---

_Comment by @codspeed-hq[bot] on 2024-07-06 15:28_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/red-knot-bench-exclude-drop)

### Merging #12218 will **improve performances by 92.47%**

<sub>Comparing <code>red-knot-bench-exclude-drop</code> (58f0244) with <code>main</code> (a62a432)</sub>



### Summary

`⚡ 3` improvements
`✅ 30` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `red-knot-bench-exclude-drop` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `red_knot_check_file[without_parse]` | 306.5 µs | 235.2 µs | +30.29% |
| ⚡ | `red_knot_check_file[incremental]` | 171.8 µs | 89.2 µs | +92.47% |
| ⚡ | `red_knot_check_file[cold]` | 388.4 µs | 317.7 µs | +22.25% |


---

_Marked ready for review by @MichaReiser on 2024-07-06 15:34_

---

_Review requested from @carljm by @MichaReiser on 2024-07-06 15:34_

---

_Merged by @MichaReiser on 2024-07-06 15:35_

---

_Closed by @MichaReiser on 2024-07-06 15:35_

---

_Branch deleted on 2024-07-06 15:35_

---

_Comment by @github-actions[bot] on 2024-07-06 15:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (error)</summary>
<p>

```
Failed to clone python-trio/trio: warning: Could not find remote branch master to clone.
fatal: Remote branch master not found in upstream origin
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
Failed to clone python-trio/trio: warning: Could not find remote branch master to clone.
fatal: Remote branch master not found in upstream origin
```

</p>
</details>




---
