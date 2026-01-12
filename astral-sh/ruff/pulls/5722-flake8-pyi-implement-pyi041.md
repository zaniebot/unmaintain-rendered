```yaml
number: 5722
title: "[`flake8-pyi`] Implement PYI041"
type: pull_request
state: merged
author: density
labels:
  - rule
assignees: []
merged: true
base: main
head: PYI041
created_at: 2023-07-13T03:18:10Z
updated_at: 2023-07-14T00:50:37Z
url: https://github.com/astral-sh/ruff/pull/5722
synced_at: 2026-01-12T15:55:19Z
```

# [`flake8-pyi`] Implement PYI041

---

_@density_

## Summary

Implements PYI041 from flake8-pyi. See [original code](https://github.com/PyCQA/flake8-pyi/blob/2a86db8271dc500247a8a69419536240334731cf/pyi.py#L1283).

This check only applies to function parameters in order to avoid issues with mypy. See https://github.com/PyCQA/flake8-pyi/issues/299.

ref: #848

## Test Plan

Snapshots, manual runs of flake8.


---

_Marked ready for review by @density on 2023-07-13 03:19_

---

_Comment by @github-actions[bot] on 2023-07-13 03:49_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+4, -0, 0 error(s))

<details><summary>airflow (+1, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/44b4a3755deea23f8ff0eb1316db880b1d64812c/airflow/decorators/__init__.pyi#L106'>airflow/decorators/__init__.pyi:106:25:</a> PYI041 Use `float` instead of `int | float`
</pre>

</p>
</details>
<details><summary>typeshed (+3, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/3c9522bcfe63599c5a2a8ee31b21a5b69f87cf3a/stdlib/random.pyi#L49'>stdlib/random.pyi:49:27:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/python/typeshed/blob/3c9522bcfe63599c5a2a8ee31b21a5b69f87cf3a/stdlib/turtle.pyi#L428'>stdlib/turtle.pyi:428:16:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/python/typeshed/blob/3c9522bcfe63599c5a2a8ee31b21a5b69f87cf3a/stdlib/turtle.pyi#L429'>stdlib/turtle.pyi:429:17:</a> PYI041 Use `float` instead of `int | float`
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PYI041 | 4 | 4 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.1±0.05ms     5.0 MB/sec    1.00      8.0±0.06ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1858.2±7.08µs     9.0 MB/sec    1.01   1872.8±8.54µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    207.7±0.49µs    14.2 MB/sec    1.01    210.3±2.96µs    14.0 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.01ms     6.3 MB/sec    1.01      4.1±0.05ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     13.7±0.09ms     3.0 MB/sec    1.02     14.0±0.10ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.02      3.4±0.02ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.03    450.3±0.70µs     6.6 MB/sec    1.00    435.6±0.84µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.05ms     4.2 MB/sec    1.00      6.1±0.03ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.02ms     6.1 MB/sec    1.03      6.9±0.03ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1450.5±5.99µs    11.5 MB/sec    1.03   1490.6±7.12µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    166.8±9.37µs    17.7 MB/sec    1.01    168.7±1.60µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.03      3.1±0.01ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.2±0.33ms     3.6 MB/sec    1.05     11.8±0.35ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.5±0.15ms     6.6 MB/sec    1.06      2.7±0.08ms     6.2 MB/sec
formatter/numpy/globals.py                 1.00   293.0±13.94µs    10.1 MB/sec    1.07   313.0±12.07µs     9.4 MB/sec
formatter/pydantic/types.py                1.00      5.5±0.23ms     4.6 MB/sec    1.05      5.8±0.16ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.04     20.7±0.66ms  2014.0 KB/sec    1.00     19.8±0.50ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.0±0.16ms     3.3 MB/sec    1.04      5.2±0.12ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   599.2±17.94µs     4.9 MB/sec    1.07   640.2±19.01µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.5±0.27ms     3.0 MB/sec    1.07      9.2±0.18ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00      9.8±0.31ms     4.1 MB/sec    1.03     10.2±0.22ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.06ms     8.0 MB/sec    1.03      2.1±0.05ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    253.1±9.29µs    11.7 MB/sec    1.06   267.9±11.92µs    11.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.15ms     5.7 MB/sec    1.03      4.6±0.12ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `rule` added by @charliermarsh on 2023-07-13 04:29_

---

_Review comment by @dhruvmanila on `crates/ruff/resources/test/fixtures/flake8_pyi/PYI041.py`:47 on 2023-07-13 06:47_

nit: I don't think these rules are checked on Python (`.py`) files

```suggestion
def f0(arg1: float | int) -> None:
    ...


def f1(arg1: float, *, arg2: float | list[str] | type[bool] | complex) -> None:
    ...


def f2(arg1: int, /, arg2: int | int | float) -> None:
    ...


def f3(arg1: int, *args: Union[int | int | float]) -> None:
    ...


async def f4(**kwargs: int | int | float) -> None:
    ...


class Foo:
    def good(self, arg: int) -> None:
        ...

    def bad(self, arg: int | float | complex) -> None:
        ...
```

---

_Review comment by @dhruvmanila on `crates/ruff/resources/test/fixtures/flake8_pyi/PYI041.pyi`:12 on 2023-07-13 06:58_

I think we should document the limitation that this rule won't check for type aliases.

---

_@dhruvmanila approved on 2023-07-13 06:59_

Looks good!

---

_Review comment by @density on `crates/ruff/resources/test/fixtures/flake8_pyi/PYI041.py`:47 on 2023-07-13 12:21_

Fixed, thanks!

---

_@density reviewed on 2023-07-13 12:21_

---

_Review comment by @density on `crates/ruff/resources/test/fixtures/flake8_pyi/PYI041.pyi`:12 on 2023-07-13 12:21_

Added a comment. Thanks!

---

_@density reviewed on 2023-07-13 12:21_

---

_Merged by @charliermarsh on 2023-07-13 16:48_

---

_Closed by @charliermarsh on 2023-07-13 16:48_

---

_Branch deleted on 2023-07-14 00:50_

---
