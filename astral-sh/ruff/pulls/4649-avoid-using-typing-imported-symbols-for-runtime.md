```yaml
number: 4649
title: Avoid using typing-imported symbols for runtime edits
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/import-context
created_at: 2023-05-25T01:14:34Z
updated_at: 2023-05-27T00:36:40Z
url: https://github.com/astral-sh/ruff/pull/4649
synced_at: 2026-01-12T03:50:03Z
```

# Avoid using typing-imported symbols for runtime edits

---

_Pull request opened by @charliermarsh on 2023-05-25 01:14_

## Summary

When we go to import a symbol (in order to use it for an autofix), we first check whether the symbol is available in the current scope. This PR ensures that when we use an existing symbol, we first verify that it was created in the appropriate context. That is, we shouldn't reuse `defaultdict` here:

```py
import typing

if typing.TYPE_CHECKING:
    from collections import defaultdict


def f(x: typing.DefaultDict[str, str]) -> None:
    ...
```

Since `typing.DefaultDict` needs to be available at runtime, as per Python's somewhat tricky rules of annotation availability. However, we _should_ reuse `defaultdict` here:

```py
import typing

if typing.TYPE_CHECKING:
    from collections import defaultdict


def f(x: "typing.DefaultDict[str, str]") -> None:
    ...
```

There's actually a more general issue here, which is that our binding tracking generally ignores preconditions for bound symbols. So, e.g., we would incorrectly reuse `defaultdict` here:

```py
import typing

if 1 > 2:
    from collections import defaultdict


def f(x: typing.DefaultDict[str, str]) -> None:
    ...
```

But, I'm just ignoring that for now.

## Test Plan

Added some additional fixtures for these specific cases.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-25 01:14_

---

_Review requested from @konstin by @charliermarsh on 2023-05-25 01:14_

---

_Comment by @github-actions[bot] on 2023-05-25 01:31_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.05     19.6±0.44ms     2.1 MB/sec    1.00     18.7±0.51ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.4±0.15ms     3.8 MB/sec    1.00      4.4±0.16ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.01   554.9±16.95µs     5.3 MB/sec    1.00   549.9±19.70µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.8±0.22ms     3.3 MB/sec    1.00      7.6±0.25ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.01      9.1±0.20ms     4.5 MB/sec    1.00      9.0±0.21ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1977.8±81.59µs     8.4 MB/sec    1.00  1953.5±46.77µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    225.8±6.61µs    13.1 MB/sec    1.02    230.9±8.73µs    12.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.12ms     6.3 MB/sec    1.02      4.1±0.16ms     6.2 MB/sec
parser/large/dataset.py                    1.00      6.9±0.13ms     5.9 MB/sec    1.01      6.9±0.13ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.01  1344.5±34.58µs    12.4 MB/sec    1.00  1328.6±49.54µs    12.5 MB/sec
parser/numpy/globals.py                    1.04    136.6±8.84µs    21.6 MB/sec    1.00    130.8±6.22µs    22.6 MB/sec
parser/pydantic/types.py                   1.01      3.0±0.11ms     8.5 MB/sec    1.00      3.0±0.08ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.7±0.05ms     2.4 MB/sec    1.00     16.7±0.05ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.02ms     3.9 MB/sec    1.00      4.3±0.02ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    444.4±4.53µs     6.6 MB/sec    1.00    443.3±4.36µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.03ms     3.6 MB/sec    1.00      7.1±0.03ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.03ms     4.8 MB/sec    1.00      8.4±0.02ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1784.3±9.69µs     9.3 MB/sec    1.00   1771.4±6.85µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.01    196.6±1.92µs    15.0 MB/sec    1.00    195.2±4.53µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.8±0.01ms     6.6 MB/sec    1.00      3.8±0.02ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.8±0.14ms     5.9 MB/sec    1.07      7.3±0.01ms     5.6 MB/sec
parser/numpy/ctypeslib.py                  1.00   1298.0±6.09µs    12.8 MB/sec    1.05   1364.9±5.78µs    12.2 MB/sec
parser/numpy/globals.py                    1.00    133.1±0.62µs    22.2 MB/sec    1.03    137.8±0.77µs    21.4 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.01ms     8.8 MB/sec    1.06      3.1±0.01ms     8.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-05-25 03:37_

---

_Review comment by @charliermarsh on `crates/ruff/src/importer/mod.rs`:241 on 2023-05-25 03:37_

This is perhaps a strange pattern, but I was finding all the `Option<Result<...>>` stuff to be really messy.

---

_Review comment by @MichaReiser on `crates/ruff/src/importer/mod.rs`:82 on 2023-05-25 06:06_

Nit: Would it make sense to define our own `Error` type here to allow callers to match on the reason or is this something that we don't need?

---

_Review comment by @MichaReiser on `crates/ruff/src/importer/mod.rs`:241 on 2023-05-25 06:07_

I like this a lot. It communicates the outcome much clearer. We could even go one step further and flatten the Option too, but I think that would make the usage more cumbersome. 

---

_@MichaReiser approved on 2023-05-25 06:09_

---

_@konstin approved on 2023-05-25 12:53_

---

_@charliermarsh reviewed on 2023-05-25 15:37_

---

_Review comment by @charliermarsh on `crates/ruff/src/importer/mod.rs`:82 on 2023-05-25 15:37_

I'm open to it -- do you have an example e.g. from Rome or elsewhere that I could use to learn how to implement that pattern?

---

_Review comment by @MichaReiser on `crates/ruff/src/importer/mod.rs`:82 on 2023-05-26 05:43_

We have one in ruff :) 

https://github.com/charliermarsh/ruff/blob/4f9d944556910f834985b9fc75f968dc6fca2557/crates/ruff_formatter/src/diagnostics.rs#L1-L162

It's normally:

* Define an `enum`
* Implement `Error` and `Display`
* Maybe: Implement `From<OtherError>` if you want automatic error conversation when using the try operator (the try operator does `ReturnErrorType::from(error_from_the_tried_operation)`

But I don't know if it's worth here since we aren't matching on the error anywhere.

---

_@MichaReiser reviewed on 2023-05-26 05:43_

---

_Review comment by @konstin on `crates/ruff/src/importer/mod.rs`:82 on 2023-05-26 07:50_

`thiserror` can also generate a bunch of the boilerplate in many cases

---

_@konstin reviewed on 2023-05-26 07:50_

---

_@charliermarsh reviewed on 2023-05-27 00:36_

---

_Review comment by @charliermarsh on `crates/ruff/src/importer/mod.rs`:82 on 2023-05-27 00:36_

I'm going to do this in a separate PR because I want to cover the `import_symbol` cases too.

---

_Merged by @charliermarsh on 2023-05-27 00:36_

---

_Closed by @charliermarsh on 2023-05-27 00:36_

---

_Branch deleted on 2023-05-27 00:36_

---
