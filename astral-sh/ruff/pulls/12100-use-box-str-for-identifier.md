```yaml
number: 12100
title: "Use `Box<str>` for `Identifier"
type: pull_request
state: closed
author: MichaReiser
labels: []
assignees: []
draft: true
base: main
head: name-box-str
created_at: 2024-06-29T09:41:40Z
updated_at: 2024-08-12T07:52:56Z
url: https://github.com/astral-sh/ruff/pull/12100
synced_at: 2026-01-12T15:55:40Z
```

# Use `Box<str>` for `Identifier

---

_@MichaReiser_

- **Use `smol_str` for Identifier**
- **Use boxed string for name**


---

_Comment by @codspeed-hq[bot] on 2024-06-29 09:46_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/name-box-str)

### Merging #12100 will **improve performances by 7.68%**

<sub>Comparing <code>name-box-str</code> (e41728a) with <code>main</code> (47b2273)</sub>



### Summary

`⚡ 4` improvements
`✅ 26` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `name-box-str` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[large/dataset.py]` | 5.4 ms | 5 ms | +7.68% |
| ⚡ | `parser[numpy/ctypeslib.py]` | 1,007.9 µs | 958.4 µs | +5.16% |
| ⚡ | `parser[pydantic/types.py]` | 2.2 ms | 2.1 ms | +5.25% |
| ⚡ | `parser[unicode/pypinyin.py]` | 329.3 µs | 314 µs | +4.85% |


---

_Comment by @MichaReiser on 2024-06-29 10:17_

Closing in favor of https://github.com/astral-sh/ruff/pull/12101 which shows the most promising results

---

_Closed by @MichaReiser on 2024-06-29 10:17_

---

_Branch deleted on 2024-08-12 07:52_

---
