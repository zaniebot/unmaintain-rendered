```yaml
number: 7581
title: Make ruff format idempotent when using stdin input
type: pull_request
state: merged
author: leiserfg
labels:
  - formatter
assignees: []
merged: true
base: main
head: fix-format-stdin
created_at: 2023-09-21T20:02:52Z
updated_at: 2023-09-21T20:50:24Z
url: https://github.com/astral-sh/ruff/pull/7581
synced_at: 2026-01-12T15:55:24Z
```

# Make ruff format idempotent when using stdin input

---

_@leiserfg_

## Summary
Currently, this happens
```sh
$ echo "print()" | ruff format - 

#Notice that nothing went to stdout
```
Which does not match `ruff check --fix - `  behavior and deletes my code every time I format it (more or less 5 times per minute :smile:).

<!-- What's the purpose of the change? What does it do, and why? -->


<!-- How was it tested? -->
I just checked that my example works as the change was very straightforward.

---

_Comment by @codspeed-hq[bot] on 2023-09-21 20:12_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/leiserfg:fix-format-stdin)

### Merging #7581 will **improve performances by 2.33%**

<sub>Comparing <code>leiserfg:fix-format-stdin</code> (839a55f) with <code>main</code> (c3774e1)</sub>



### Summary

`⚡ 1` improvements
`✅ 24` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `leiserfg:fix-format-stdin` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[numpy/ctypeslib.py]` | 35 ms | 34.2 ms | +2.33% |


---

_Comment by @github-actions[bot] on 2023-09-21 20:18_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@charliermarsh approved on 2023-09-21 20:50_

Thank you! This makes sense.

---

_Label `formatter` added by @charliermarsh on 2023-09-21 20:50_

---

_Merged by @charliermarsh on 2023-09-21 20:50_

---

_Closed by @charliermarsh on 2023-09-21 20:50_

---
