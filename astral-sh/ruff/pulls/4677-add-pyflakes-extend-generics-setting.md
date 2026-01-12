```yaml
number: 4677
title: "Add `pyflakes.extend-generics` setting"
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: add-pyflakes-extend-annotated-subscripts
created_at: 2023-05-27T09:49:10Z
updated_at: 2023-06-02T09:36:52Z
url: https://github.com/astral-sh/ruff/pull/4677
synced_at: 2026-01-12T15:55:16Z
```

# Add `pyflakes.extend-generics` setting

---

_@JonathanPlasse_

- Close #4654
- [ ] Improve documentation
<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This setting has been added to fix a false positive where an object should be considered an annotated subscript (c.f. #4654).

## Test Plan

A fixture has been added that tests with and without the new setting.


---

_Comment by @github-actions[bot] on 2023-05-27 10:01_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.0±0.06ms     2.4 MB/sec    1.00     16.9±0.08ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.01ms     4.1 MB/sec    1.00      4.1±0.01ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    511.4±1.42µs     5.8 MB/sec    1.00    513.5±2.86µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.03ms     3.6 MB/sec    1.00      7.1±0.02ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.02ms     5.0 MB/sec    1.00      8.2±0.02ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1811.7±5.98µs     9.2 MB/sec    1.01   1826.8±2.82µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    205.3±0.41µs    14.4 MB/sec    1.02    210.0±2.05µs    14.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.01ms     6.8 MB/sec    1.00      3.7±0.01ms     6.8 MB/sec
parser/large/dataset.py                    1.16      7.3±0.00ms     5.6 MB/sec    1.00      6.3±0.00ms     6.5 MB/sec
parser/numpy/ctypeslib.py                  1.14   1394.4±0.96µs    11.9 MB/sec    1.00   1224.8±0.84µs    13.6 MB/sec
parser/numpy/globals.py                    1.11    139.8±0.19µs    21.1 MB/sec    1.00    126.2±0.24µs    23.4 MB/sec
parser/pydantic/types.py                   1.14      3.1±0.00ms     8.3 MB/sec    1.00      2.7±0.00ms     9.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.4±0.25ms     2.3 MB/sec    1.00     17.4±0.23ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.08ms     3.8 MB/sec    1.01      4.4±0.09ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   510.3±13.31µs     5.8 MB/sec    1.01   513.3±12.74µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.13ms     3.5 MB/sec    1.00      7.3±0.31ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.01      8.4±0.12ms     4.8 MB/sec    1.00      8.4±0.08ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1803.2±24.97µs     9.2 MB/sec    1.00  1798.3±22.62µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    204.2±3.99µs    14.4 MB/sec    1.06   216.2±19.85µs    13.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.7 MB/sec    1.01      3.8±0.05ms     6.7 MB/sec
parser/large/dataset.py                    1.01      6.6±0.09ms     6.1 MB/sec    1.00      6.5±0.06ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.01  1247.6±23.31µs    13.3 MB/sec    1.00  1229.2±21.13µs    13.5 MB/sec
parser/numpy/globals.py                    1.00    125.3±1.73µs    23.6 MB/sec    1.00    125.4±2.14µs    23.5 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.1 MB/sec    1.00      2.8±0.04ms     9.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@JonathanPlasse reviewed on 2023-05-27 21:29_

---

_Review comment by @JonathanPlasse on `crates/ruff_python_semantic/src/analyze/typing.rs`:41 on 2023-05-27 21:29_

Should this be computed in the setting initialization?
I copied `extend_immutable_calls` implementation.

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/analyze/typing.rs`:41 on 2023-05-30 07:03_

Do we need the `collect` call here? It seems that we only iterate once. 

---

_@MichaReiser reviewed on 2023-05-30 07:03_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-05-30 07:03_

---

_@JonathanPlasse reviewed on 2023-05-30 11:38_

---

_Review comment by @JonathanPlasse on `crates/ruff_python_semantic/src/analyze/typing.rs`:41 on 2023-05-30 11:38_

Changed it.

---

_Renamed from "Add pyflakes.extend-annotated-subscripts new setting" to "Add `pyflakes.extend-generics` setting" by @charliermarsh on 2023-06-01 22:11_

---

_Merged by @charliermarsh on 2023-06-01 22:19_

---

_Closed by @charliermarsh on 2023-06-01 22:19_

---

_Comment by @charliermarsh on 2023-06-01 22:24_

@JonathanPlasse - I decided to rename this, since `annotated-subscripts` is more of an internal concept.

---

_Review comment by @last-partizan on `crates/ruff/src/rules/pyflakes/snapshots/ruff__rules__pyflakes__tests__F401_F401_15.py.snap`:20 on 2023-06-02 08:01_

I'm little late for the party, but isn't this PR supposed to eliminate this error?

I'm trying to test it, with following config:

```toml
[pyflakes]
extend-generics = ["django.db.models.ForeignKey"]
```

```
> cargo r --bin ruff -- crates/ruff/resources/test/fixtures/pyflakes/F401_15.py
...
crates/ruff/resources/test/fixtures/pyflakes/F401_15.py:5:25: F401 [*] `pathlib.Path` imported but unused
```

---

_@last-partizan reviewed on 2023-06-02 08:01_

---

_Branch deleted on 2023-06-02 08:19_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/pyflakes/snapshots/ruff__rules__pyflakes__tests__F401_F401_15.py.snap`:20 on 2023-06-02 08:30_

This works if you put `ruff.toml` in the same folder as `F401_15.py`.

---

_@JonathanPlasse reviewed on 2023-06-02 08:30_

---

_Review comment by @last-partizan on `crates/ruff/src/rules/pyflakes/snapshots/ruff__rules__pyflakes__tests__F401_F401_15.py.snap`:20 on 2023-06-02 09:36_

Oh, great, thank you!

---

_@last-partizan reviewed on 2023-06-02 09:36_

---
