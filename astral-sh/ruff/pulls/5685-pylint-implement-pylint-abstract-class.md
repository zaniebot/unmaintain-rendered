```yaml
number: 5685
title: "[`pylint`] Implement Pylint `abstract-class-instantiated` (`E0110`)"
type: pull_request
state: closed
author: silvanocerza
labels: []
assignees: []
draft: true
base: main
head: abstract_class_instantiated
created_at: 2023-07-11T12:42:51Z
updated_at: 2023-10-19T16:13:52Z
url: https://github.com/astral-sh/ruff/pull/5685
synced_at: 2026-01-12T02:32:41Z
```

# [`pylint`] Implement Pylint `abstract-class-instantiated` (`E0110`)

---

_Pull request opened by @silvanocerza on 2023-07-11 12:42_

## Summary

Relates to #970.

I made a draft PR as this doesn't fully implement the `abstract-class-instantiated` rule.

I didn't implement it fully as I couldn't find a reliable way of getting `StmtClassDef` of imported classes and builtins.

That makes impossible to verify if classes that inherit from `abc.ABC` and have abstract methods are abstract as we can't find definition of `abc.ABC` in the `SemanticModel`'s `definitions`. 

`FifthBadClass` in `abstract_class_instantiated.py` is the perfect example for this limitation.

## Test Plan

Added new snapshot.

`abstract_class_instantiated.py` has been adapted from `pylint`'s test file that is used to verify this same rule. All errors, apart from `FifthBadClass` as specified above, that `pylint` reports are also reported by `ruff`.


---

_Renamed from "[`pylint`] Implement Pylint abstract-class-instantiated (`E0110`)" to "[`pylint`] Implement Pylint `abstract-class-instantiated` (`E0110`)" by @silvanocerza on 2023-07-11 12:43_

---

_Comment by @github-actions[bot] on 2023-07-11 13:34_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.3±0.36ms     3.9 MB/sec    1.21     12.5±0.65ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.4±0.18ms     7.0 MB/sec    1.12      2.7±0.18ms     6.3 MB/sec
formatter/numpy/globals.py                 1.00   277.9±20.04µs    10.6 MB/sec    1.03   285.4±20.19µs    10.3 MB/sec
formatter/pydantic/types.py                1.00      5.0±0.21ms     5.1 MB/sec    1.14      5.8±0.35ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.00     19.5±0.78ms     2.1 MB/sec    1.03     20.0±0.91ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.5±0.22ms     3.7 MB/sec    1.06      4.7±0.23ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.05   612.8±62.82µs     4.8 MB/sec    1.00   583.1±25.52µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.2±0.35ms     3.1 MB/sec    1.50     12.3±4.74ms     2.1 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.38ms     4.7 MB/sec    1.22     10.5±0.47ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1938.9±90.59µs     8.6 MB/sec    1.11      2.2±0.11ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    229.8±9.77µs    12.8 MB/sec    1.09   250.9±18.64µs    11.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.24ms     6.3 MB/sec    1.14      4.6±0.27ms     5.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.06     12.8±0.56ms     3.2 MB/sec    1.00     12.1±0.43ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.04      2.9±0.24ms     5.7 MB/sec    1.00      2.8±0.14ms     6.0 MB/sec
formatter/numpy/globals.py                 1.01   317.9±22.57µs     9.3 MB/sec    1.00   313.9±20.94µs     9.4 MB/sec
formatter/pydantic/types.py                1.00      6.1±0.26ms     4.2 MB/sec    1.00      6.1±0.29ms     4.2 MB/sec
linter/all-rules/large/dataset.py          1.01     21.1±0.61ms  1969.7 KB/sec    1.00     21.0±0.50ms  1988.2 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      5.6±0.26ms     3.0 MB/sec    1.00      5.5±0.28ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.04   673.5±42.73µs     4.4 MB/sec    1.00   648.1±28.43µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.01      9.6±0.45ms     2.7 MB/sec    1.00      9.5±0.35ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.02     10.8±0.63ms     3.8 MB/sec    1.00     10.6±0.36ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.2±0.12ms     7.4 MB/sec    1.00      2.2±0.10ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.03   267.6±13.45µs    11.0 MB/sec    1.00   260.6±11.94µs    11.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.7±0.25ms     5.4 MB/sec    1.00      4.7±0.17ms     5.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-07-13 00:10_

I think this may be too hard to support (yet) due to the limitations you describe: we don't have any way to resolve symbols across files. I've been thinking a bit about this problem lately, we have many of the pieces that we'd need to support it, but it does require a pretty significant rethinking of Ruff's overall compilation pipeline.

---

_Comment by @silvanocerza on 2023-07-13 09:21_

I thought the same, I just realized way too late while implementing this. 😅 
I'll keep the PR open in any case, as soon as we have the possibility to implement it properly I'll update it.

---

_Label `multifile-analysis` added by @zanieb on 2023-10-19 16:13_

---

_Comment by @zanieb on 2023-10-19 16:13_

I'm going to close this since multifile analysis isn't something I see happening soon.

Thanks for contributing though! I hope this is useful once we add the necessary functionality.

---

_Closed by @zanieb on 2023-10-19 16:13_

---
