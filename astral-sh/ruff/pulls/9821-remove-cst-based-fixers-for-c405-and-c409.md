```yaml
number: 9821
title: "Remove CST-based fixers for `C405` and `C409`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - fixes
assignees: []
merged: true
base: main
head: charlie/lcst2
created_at: 2024-02-05T02:02:18Z
updated_at: 2024-02-05T02:24:38Z
url: https://github.com/astral-sh/ruff/pull/9821
synced_at: 2026-01-12T15:55:30Z
```

# Remove CST-based fixers for `C405` and `C409`

---

_@charliermarsh_

_No description provided._

---

_Label `fixes` added by @charliermarsh on 2024-02-05 02:02_

---

_Marked ready for review by @charliermarsh on 2024-02-05 02:04_

---

_Merged by @charliermarsh on 2024-02-05 02:17_

---

_Closed by @charliermarsh on 2024-02-05 02:17_

---

_Branch deleted on 2024-02-05 02:17_

---

_Comment by @codspeed-hq[bot] on 2024-02-05 02:18_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/lcst2)

### Merging #9821 will **improve performances by 8.42%**

<sub>Comparing <code>charlie/lcst2</code> (cb00129) with <code>main</code> (c5fa0cc)</sub>



### Summary

`⚡ 2` improvements
`✅ 28` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/lcst2` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[unicode/pypinyin.py]` | 11.6 ms | 10.7 ms | +8.42% |
| ⚡ | `linter/all-with-preview-rules[unicode/pypinyin.py]` | 12.6 ms | 11.6 ms | +8.38% |


---

_Comment by @github-actions[bot] on 2024-02-05 02:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB113
	- FURB131
	- FURB132
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
