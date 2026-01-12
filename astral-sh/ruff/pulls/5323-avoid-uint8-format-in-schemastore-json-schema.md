```yaml
number: 5323
title: "Avoid `uint8` format in SchemaStore JSON Schema"
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
base: main
head: charlie/uint8
created_at: 2023-06-23T03:30:50Z
updated_at: 2023-06-25T21:51:47Z
url: https://github.com/astral-sh/ruff/pull/5323
synced_at: 2026-01-12T15:55:18Z
```

# Avoid `uint8` format in SchemaStore JSON Schema

---

_@charliermarsh_

The new `tabSize` property uses `uint8`, but SchemaStore rejects it. (I edited the generated PR manually.)

---

_Review requested from @konstin by @charliermarsh on 2023-06-23 03:30_

---

_Comment by @github-actions[bot] on 2023-06-23 03:57_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.13      7.5±0.01ms     5.4 MB/sec    1.00      6.6±0.01ms     6.2 MB/sec
formatter/numpy/ctypeslib.py               1.10   1590.9±2.91µs    10.5 MB/sec    1.00   1448.6±2.88µs    11.5 MB/sec
formatter/numpy/globals.py                 1.06    172.5±0.29µs    17.1 MB/sec    1.00    162.7±1.68µs    18.1 MB/sec
formatter/pydantic/types.py                1.11      3.7±0.01ms     6.9 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
linter/all-rules/large/dataset.py          1.00     13.0±0.05ms     3.1 MB/sec    1.00     13.0±0.06ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.04    440.8±1.44µs     6.7 MB/sec    1.00    424.7±0.75µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.01      6.6±0.02ms     6.2 MB/sec    1.00      6.5±0.01ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1454.2±1.71µs    11.5 MB/sec    1.00   1450.0±2.25µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.2±0.24µs    18.1 MB/sec    1.00    163.2±0.24µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.00ms     8.5 MB/sec    1.00      3.0±0.00ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.2±0.44ms     4.4 MB/sec    1.01      9.3±0.41ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00  1952.2±92.00µs     8.5 MB/sec    1.08      2.1±0.19ms     7.9 MB/sec
formatter/numpy/globals.py                 1.00   225.3±17.96µs    13.1 MB/sec    1.04   234.1±19.26µs    12.6 MB/sec
formatter/pydantic/types.py                1.00      4.5±0.19ms     5.7 MB/sec    1.04      4.6±0.23ms     5.5 MB/sec
linter/all-rules/large/dataset.py          1.00     17.6±0.62ms     2.3 MB/sec    1.01     17.9±0.56ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.7±0.19ms     3.5 MB/sec    1.00      4.7±0.21ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   577.0±44.71µs     5.1 MB/sec    1.01   580.0±32.14µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.8±0.29ms     3.2 MB/sec    1.02      8.0±0.26ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      9.3±0.37ms     4.4 MB/sec    1.01      9.4±0.43ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1976.9±87.50µs     8.4 MB/sec    1.01      2.0±0.11ms     8.3 MB/sec
linter/default-rules/numpy/globals.py      1.03   243.6±12.44µs    12.1 MB/sec    1.00   237.6±13.22µs    12.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.2±0.16ms     6.1 MB/sec    1.00      4.2±0.20ms     6.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin requested changes on 2023-06-23 06:31_

Could we edit https://github.com/SchemaStore/schemastore/blob/144cb57a0f815b909f1b8baf3bb7e6ac4aabd1a7/src/schema-validation.json#L714 instead? It seems that `format` is for custom extensions and implementations are allowed to treat it the way they like (https://json-schema.org/draft/2019-09/json-schema-validation.html#rfc.section.7.2.3), and schemastore makes use of that liberally.

---

_Comment by @charliermarsh on 2023-06-25 21:49_

Yes good call.

---

_Closed by @charliermarsh on 2023-06-25 21:49_

---

_Comment by @charliermarsh on 2023-06-25 21:51_

https://github.com/SchemaStore/schemastore/pull/3034

---
