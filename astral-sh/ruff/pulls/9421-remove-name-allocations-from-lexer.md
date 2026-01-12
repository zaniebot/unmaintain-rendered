```yaml
number: 9421
title: "Remove `Name` allocations from lexer"
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
draft: true
base: main
head: charlie/name
created_at: 2024-01-07T15:13:52Z
updated_at: 2024-01-07T15:48:30Z
url: https://github.com/astral-sh/ruff/pull/9421
synced_at: 2026-01-12T15:55:28Z
```

# Remove `Name` allocations from lexer

---

_@charliermarsh_

I don't expect this to make a difference in the parser, since we still perform the same allocation in the parser now rather than the lexer, but am still curious to see the benchmarks.

---

_Comment by @codspeed-hq[bot] on 2024-01-07 15:29_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/name)

### Merging #9421 will **improve performances by 11.14%**

<sub>Comparing <code>charlie/name</code> (7f199a2) with <code>main</code> (6395343)</sub>



### Summary

`⚡ 5` improvements
`✅ 25` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/name` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `lexer[unicode/pypinyin.py]` | 545.7 µs | 502.4 µs | +8.62% |
| ⚡ | `lexer[numpy/globals.py]` | 216.1 µs | 207.3 µs | +4.24% |
| ⚡ | `lexer[large/dataset.py]` | 8.4 ms | 7.6 ms | +11.14% |
| ⚡ | `lexer[pydantic/types.py]` | 3.7 ms | 3.4 ms | +10.05% |
| ⚡ | `lexer[numpy/ctypeslib.py]` | 1.8 ms | 1.6 ms | +9.25% |


---

_Comment by @charliermarsh on 2024-01-07 15:35_

Good improvement in the lexer, but no change in the parser. I'd actually be curious if the parser got _worse_ or not? The parser benchmark includes lexing right now.

---

_Comment by @charliermarsh on 2024-01-07 15:38_

It looks like parsing did get a bit slower but didn't meet our reporting threshold. So this probably isn't worth doing right now.

---

_Closed by @charliermarsh on 2024-01-07 15:38_

---

_Renamed from "Rename `Name` allocations from lexer" to "Remove `Name` allocations from lexer" by @charliermarsh on 2024-01-07 15:46_

---

_Comment by @charliermarsh on 2024-01-07 15:48_

\cc @MichaReiser just for fun

---
