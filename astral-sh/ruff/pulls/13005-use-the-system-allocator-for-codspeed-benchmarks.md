```yaml
number: 13005
title: Use the system allocator for codspeed benchmarks
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: use-system-allocator-for-codspeed
created_at: 2024-08-20T08:20:40Z
updated_at: 2024-08-21T06:46:53Z
url: https://github.com/astral-sh/ruff/pull/13005
synced_at: 2026-01-10T21:38:32Z
```

# Use the system allocator for codspeed benchmarks

---

_Pull request opened by @MichaReiser on 2024-08-20 08:20_

## Summary

Our benchmarks have been very flaky recently. The one thing that I noticed is that it is mainly due to changes where jemalloc performs reallocations (or moves entire arenas). This *randomness* is intentional by jemalloc but is kind of hurtful in "reliable" benchmarks. 

I haven't found a way to disable the randomness. This PR suggests disabling jemalloc for benchmarks running in codspeed. This has the downside that the codspeed benchmarks are further away from what we run in production. I do think that this is the less worse outcome than us loosing trust in our benchmarks (I'm currently ignoring them because they're just noise). 

[Codspeed support thread](https://discord.com/channels/1065233827569598464/1211340667809169530/1220074050106167357)

## Test plan

We'll have to test on main to see if the benchmark results are more stable.

---

_Label `ci` added by @MichaReiser on 2024-08-20 08:20_

---

_Comment by @codspeed-hq[bot] on 2024-08-20 08:26_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/use-system-allocator-for-codspeed)

### Merging #13005 will **degrade performances by 11.91%**

<sub>Comparing <code>use-system-allocator-for-codspeed</code> (e367f53) with <code>main</code> (c65e331)</sub>



### Summary

`⚡ 1` improvements
`❌ 10 (👁 10)` regressions
`✅ 21` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `use-system-allocator-for-codspeed` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `linter/all-rules[pydantic/types.py]` | 8.3 ms | 8.7 ms | -4.1% |
| 👁 | `linter/default-rules[large/dataset.py]` | 3.7 ms | 3.9 ms | -5.64% |
| ⚡ | `linter/default-rules[numpy/globals.py]` | 188.9 µs | 171 µs | +10.47% |
| 👁 | `linter/default-rules[pydantic/types.py]` | 1.8 ms | 1.9 ms | -4.24% |
| 👁 | `linter/all-with-preview-rules[pydantic/types.py]` | 9.8 ms | 10.3 ms | -4.79% |
| 👁 | `linter/all-with-preview-rules[unicode/pypinyin.py]` | 2.4 ms | 2.5 ms | -4.03% |
| 👁 | `parser[large/dataset.py]` | 4.9 ms | 5.6 ms | -11.4% |
| 👁 | `parser[numpy/ctypeslib.py]` | 910.7 µs | 998.5 µs | -8.79% |
| 👁 | `parser[numpy/globals.py]` | 101.5 µs | 112.7 µs | -9.92% |
| 👁 | `parser[pydantic/types.py]` | 2 ms | 2.1 ms | -6.31% |
| 👁 | `parser[unicode/pypinyin.py]` | 308.9 µs | 350.6 µs | -11.91% |


---

_Comment by @github-actions[bot] on 2024-08-20 08:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @MichaReiser on 2024-08-20 08:49_

---

_Comment by @MichaReiser on 2024-08-21 06:46_

I'm going to merge this. We can still revert if it doesn't help to make the benchmarks more stable.

---

_Merged by @MichaReiser on 2024-08-21 06:46_

---

_Closed by @MichaReiser on 2024-08-21 06:46_

---

_Branch deleted on 2024-08-21 06:46_

---
