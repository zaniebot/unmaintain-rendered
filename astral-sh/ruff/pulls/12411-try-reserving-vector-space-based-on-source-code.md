```yaml
number: 12411
title: Try reserving vector space based on source code estimate
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
draft: true
base: main
head: charlie/reserve
created_at: 2024-07-19T19:54:18Z
updated_at: 2024-08-06T02:29:49Z
url: https://github.com/astral-sh/ruff/pull/12411
synced_at: 2026-01-10T21:47:02Z
```

# Try reserving vector space based on source code estimate

---

_Pull request opened by @charliermarsh on 2024-07-19 19:54_

Was curious so I hacked this together quickly. I see basically no improvement with Hyperfine and don't expect a CodSpeed improvement, but lets see.

---

_Comment by @codspeed-hq[bot] on 2024-07-19 19:59_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/reserve)

### Merging #12411 will **improve performances by 13.64%**

<sub>Comparing <code>charlie/reserve</code> (2fdde07) with <code>main</code> (ca22248)</sub>



### Summary

`⚡ 4` improvements
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/reserve` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[large/dataset.py]` | 3.7 ms | 3.4 ms | +8.72% |
| ⚡ | `linter/default-rules[numpy/ctypeslib.py]` | 906.9 µs | 798 µs | +13.64% |
| ⚡ | `linter/default-rules[numpy/globals.py]` | 183.4 µs | 163.8 µs | +11.91% |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 1.9 ms | 1.7 ms | +10.6% |


---

_Comment by @github-actions[bot] on 2024-07-19 20:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @charliermarsh on 2024-07-19 20:26_

I don't see this improvement at all locally, I'm worried it's fake!

---

_Closed by @charliermarsh on 2024-08-06 02:29_

---
