```yaml
number: 7950
title: When only unsafe fixes are available, include note that no fixes are available first
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/hidden-lonely
created_at: 2023-10-13T16:16:02Z
updated_at: 2023-10-13T17:43:15Z
url: https://github.com/astral-sh/ruff/pull/7950
synced_at: 2026-01-12T02:32:41Z
```

# When only unsafe fixes are available, include note that no fixes are available first

---

_Pull request opened by @zanieb on 2023-10-13 16:16_

I believe this is a bit clearer.

When no fixes are available (safe _and_ unsafe) we will not include a message at all.

---

_Comment by @codspeed-hq[bot] on 2023-10-13 16:29_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/zanie/hidden-lonely)

### Merging #7950 will **improve performances by 4.12%**

<sub>Comparing <code>zanie/hidden-lonely</code> (613f8a6) with <code>main</code> (8255e4e)</sub>



### Summary

`⚡ 1` improvements
`✅ 24` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `zanie/hidden-lonely` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 36.9 ms | 35.4 ms | +4.12% |


---

_Comment by @github-actions[bot] on 2023-10-13 16:32_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+2, -2, 0 error(s))

<details><summary>docker-py (+1, -1)</summary>
<p>

<pre>
- 1 hidden fix can be enabled with the `--unsafe-fixes` option.
+ No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
</pre>

</p>
</details>
<details><summary>reflex (+1, -1)</summary>
<p>

<pre>
- 1 hidden fix can be enabled with the `--unsafe-fixes` option.
+ No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
</pre>

</p>
</details>



---

_@charliermarsh approved on 2023-10-13 17:04_

---

_Merged by @zanieb on 2023-10-13 17:43_

---

_Closed by @zanieb on 2023-10-13 17:43_

---

_Branch deleted on 2023-10-13 17:43_

---
