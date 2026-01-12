```yaml
number: 6268
title: "FA102: add configuration to always require `annotations` future when annotations used"
type: pull_request
state: closed
author: akx
labels: []
assignees: []
base: main
head: ann-future-required
created_at: 2023-08-02T08:37:08Z
updated_at: 2023-09-15T01:15:19Z
url: https://github.com/astral-sh/ruff/pull/6268
synced_at: 2026-01-12T02:39:09Z
```

# FA102: add configuration to always require `annotations` future when annotations used

---

_Pull request opened by @akx on 2023-08-02 08:37_

## Summary

This PR extends the FA102 lint with a configuration that makes it yell whenever type annotations are used in a file that doesn't have the import.

It's currently a bit loud since it complains about every annotation in a non-compliant file – and an autofix to add the import would be nice.

## Test Plan

* New test fixtures within.
* Tested against some production code (Found 7290 errors.)

---

_Comment by @github-actions[bot] on 2023-08-02 09:07_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.5±0.18ms     4.3 MB/sec    1.00      9.4±0.19ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.01  1885.0±43.57µs     8.8 MB/sec    1.00  1871.6±44.89µs     8.9 MB/sec
formatter/numpy/globals.py                 1.02    212.9±3.87µs    13.9 MB/sec    1.00    209.2±6.95µs    14.1 MB/sec
formatter/pydantic/types.py                1.01      4.1±0.09ms     6.3 MB/sec    1.00      4.0±0.06ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.02     12.9±0.24ms     3.2 MB/sec    1.00     12.6±0.25ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.3±0.04ms     5.1 MB/sec    1.00      3.2±0.05ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.02   445.7±10.27µs     6.6 MB/sec    1.00    438.4±9.57µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.7±0.10ms     4.4 MB/sec    1.00      5.7±0.09ms     4.5 MB/sec
linter/default-rules/large/dataset.py      1.01      6.7±0.13ms     6.0 MB/sec    1.00      6.7±0.12ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1417.5±26.50µs    11.7 MB/sec    1.00  1396.1±23.66µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.04    156.3±3.81µs    18.9 MB/sec    1.00    150.5±2.96µs    19.6 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.0±0.05ms     8.5 MB/sec    1.00      3.0±0.05ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.08ms     4.2 MB/sec    1.00      9.8±0.13ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1899.8±30.12µs     8.8 MB/sec    1.00  1904.5±19.42µs     8.7 MB/sec
formatter/numpy/globals.py                 1.00    211.5±4.54µs    14.0 MB/sec    1.01    213.8±5.43µs    13.8 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.05ms     6.1 MB/sec    1.01      4.2±0.04ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.2±0.13ms     3.1 MB/sec    1.00     13.2±0.10ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.03ms     4.8 MB/sec    1.00      3.5±0.03ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    432.6±5.06µs     6.8 MB/sec    1.00    429.0±7.32µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.0±0.12ms     4.3 MB/sec    1.00      5.9±0.09ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.09ms     5.6 MB/sec    1.00      7.2±0.06ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1476.4±23.41µs    11.3 MB/sec    1.00  1471.7±16.24µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.02    168.2±3.34µs    17.5 MB/sec    1.00    164.4±2.45µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.04ms     8.1 MB/sec    1.00      3.2±0.03ms     8.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @konstin on 2023-08-03 09:55_

Could you clarify with kinds of errors are you trying to catch with this setting? E.g.
```python
def main(arg: int) -> str:
    pass
```
should be valid in all supported python versions.

---

_Comment by @akx on 2023-08-03 09:56_

> Could you clarify with kinds of errors are you trying to catch with this setting? E.g.

Well, e.g.
```python
class Foo:
   def copy(self) -> Foo:
       ...
```
would ordinarily require `-> "Foo"`.

This configuration would ensure consistency in all files that use annotations in the first place.

---

_Comment by @charliermarsh on 2023-08-03 22:56_

What's the motivation for this over setting [`required-imports`](https://beta.ruff.rs/docs/settings/#isort-required-imports)? Is it that you want to avoid `__future__` in some files?

---

_Comment by @akx on 2023-08-06 16:11_

> What's the motivation for this over setting [`required-imports`](https://beta.ruff.rs/docs/settings/#isort-required-imports)? Is it that you want to avoid `__future__` in some files?

Yeah, pretty much that – I wouldn't want to litter files with an import that isn't required for no good reason (and the idea here is to introduce this to a project that already has quite some history).

I totally understand if this is too niche – feel free to close if you think it's out of scope :)

---

_Comment by @charliermarsh on 2023-09-15 01:15_

Sorry for the delay here @akx. I think I'm gonna err on the side of "too niche" for now -- this is definitely reasonable, but I want to limit the number of ways to accomplish this, and I think `required-imports` is already a passable solution, since it doesn't add `__future__` to empty files, and should only really _affect_ files that make use of annotations anyway.

---

_Closed by @charliermarsh on 2023-09-15 01:15_

---
