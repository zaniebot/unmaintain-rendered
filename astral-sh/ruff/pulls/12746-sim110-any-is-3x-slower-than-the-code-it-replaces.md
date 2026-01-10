```yaml
number: 12746
title: "SIM110: `any()` is ~3x slower than the code it replaces"
type: pull_request
state: merged
author: cclauss
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-08-08T09:09:01Z
updated_at: 2024-08-08T15:19:59Z
url: https://github.com/astral-sh/ruff/pull/12746
synced_at: 2026-01-10T21:47:02Z
```

# SIM110: `any()` is ~3x slower than the code it replaces

---

_Pull request opened by @cclauss on 2024-08-08 09:09_

> ~Builtins are also more efficient than `for` loops.~

Let's not promise performance because this code transformation does not deliver.

Benchmark written by @dcbaker

> `any()` seems to be about 1/3 as fast (Python 3.11.9, NixOS):
```python
loop = 'abcdef'.split()
found = 'f'
nfound = 'g'


def test1():
    for x in loop:
        if x == found:
            return True
    return False


def test2():
    return any(x == found for x in loop)


def test3():
    for x in loop:
        if x == nfound:
            return True
    return False


def test4():
    return any(x == nfound for x in loop)


if __name__ == "__main__":
    import timeit

    print('for loop (found)    :', timeit.timeit(test1))
    print('for loop (not found):', timeit.timeit(test3))
    print('any() (found)       :', timeit.timeit(test2))
    print('any() (not found)   :', timeit.timeit(test4))
```
```
for loop (found)    : 0.051076093994197436
for loop (not found): 0.04388196699437685
any() (found)       : 0.15422860698890872
any() (not found)   : 0.15568504799739458
```
I have retested with longer lists and on multiple Python versions with similar results.

---

_Comment by @codspeed-hq[bot] on 2024-08-08 09:14_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cclauss:patch-1)

### Merging #12746 will **degrade performances by 6.3%**

<sub>Comparing <code>cclauss:patch-1</code> (8fbf9d1) with <code>main</code> (6d9205e)</sub>



### Summary

`⚡ 1` improvements
`❌ 1` regressions
`✅ 30` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/cclauss:patch-1)._

### Benchmarks breakdown

|     | Benchmark | `main` | `cclauss:patch-1` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[numpy/globals.py]` | 725.1 µs | 773.8 µs | -6.3% |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 1.9 ms | 1.8 ms | +4.12% |


---

_Comment by @github-actions[bot] on 2024-08-08 09:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "SIM110: `any()` is ~3x less performant than the code it replaces" to "SIM110: `any()` is ~3x slower than the code it replaces" by @cclauss on 2024-08-08 09:31_

---

_@charliermarsh approved on 2024-08-08 12:25_

---

_Merged by @charliermarsh on 2024-08-08 12:25_

---

_Closed by @charliermarsh on 2024-08-08 12:25_

---

_Label `documentation` added by @charliermarsh on 2024-08-08 12:25_

---

_Branch deleted on 2024-08-08 15:19_

---
