```yaml
number: 6136
title: "[`flake8-pyi`] Implement PYI049"
type: pull_request
state: merged
author: LaBatata101
labels:
  - rule
assignees: []
merged: true
base: main
head: PYI049
created_at: 2023-07-27T22:01:09Z
updated_at: 2023-07-29T01:01:01Z
url: https://github.com/astral-sh/ruff/pull/6136
synced_at: 2026-01-12T02:58:30Z
```

# [`flake8-pyi`] Implement PYI049

---

_Pull request opened by @LaBatata101 on 2023-07-27 22:01_

## Summary

Checks for the presence of unused private `typing.TypedDict` definitions.

ref #848 

## Test Plan

Snapshots and manual runs of flake8


---

_Comment by @github-actions[bot] on 2023-07-27 22:15_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+2, -0, 0 error(s))

<details><summary>typeshed (+2, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/0d8a6bc2004b7b6d8335e35b017438125c305264/stubs/pycocotools/pycocotools/__init__.pyi#L4'>stubs/pycocotools/pycocotools/__init__.pyi:4:7:</a> PYI049 Private TypedDict `_EncodedRLE` is never used
+ <a href='https://github.com/python/typeshed/blob/0d8a6bc2004b7b6d8335e35b017438125c305264/stubs/python-xlib/Xlib/display.pyi#L25'>stubs/python-xlib/Xlib/display.pyi:25:7:</a> PYI049 Private TypedDict `_ResourceBaseClassesType` is never used
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PYI049 | 2 | 2 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.7±0.13ms     4.2 MB/sec    1.01      9.8±0.21ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.01  1927.8±24.13µs     8.6 MB/sec    1.00  1915.2±22.15µs     8.7 MB/sec
formatter/numpy/globals.py                 1.01    215.4±9.57µs    13.7 MB/sec    1.00    212.8±2.45µs    13.9 MB/sec
formatter/pydantic/types.py                1.01      4.2±0.05ms     6.1 MB/sec    1.00      4.1±0.06ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.04     12.8±0.17ms     3.2 MB/sec    1.00     12.4±0.26ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.2±0.04ms     5.1 MB/sec    1.00      3.2±0.06ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    440.2±8.18µs     6.7 MB/sec    1.01    443.5±6.80µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.14ms     4.4 MB/sec    1.01      5.8±0.08ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.02      7.0±0.15ms     5.8 MB/sec    1.00      6.9±0.07ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1459.7±20.06µs    11.4 MB/sec    1.00  1445.0±12.68µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    156.7±3.41µs    18.8 MB/sec    1.02    160.0±7.07µs    18.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.05ms     8.4 MB/sec    1.00      3.0±0.04ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     13.0±0.47ms     3.1 MB/sec    1.07     13.8±0.54ms     2.9 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.4±0.13ms     6.8 MB/sec    1.06      2.6±0.12ms     6.5 MB/sec
formatter/numpy/globals.py                 1.00   287.8±17.66µs    10.3 MB/sec    1.06   303.8±18.00µs     9.7 MB/sec
formatter/pydantic/types.py                1.00      5.5±0.30ms     4.6 MB/sec    1.04      5.8±0.22ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.07     18.6±0.75ms     2.2 MB/sec    1.00     17.3±0.85ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.7±0.17ms     3.5 MB/sec    1.00      4.7±0.47ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   566.7±21.36µs     5.2 MB/sec    1.05   592.5±29.28µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.2±0.24ms     3.1 MB/sec    1.00      8.1±0.28ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.02     10.5±0.35ms     3.9 MB/sec    1.00     10.2±0.35ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03      2.1±0.08ms     8.0 MB/sec    1.00      2.0±0.07ms     8.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    241.1±9.65µs    12.2 MB/sec    1.03   247.3±12.88µs    11.9 MB/sec
linter/default-rules/pydantic/types.py     1.03      4.6±0.14ms     5.6 MB/sec    1.00      4.4±0.15ms     5.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @LaBatata101 on 2023-07-27 22:23_

This also seems to have false positives like in #6134 

---

_Label `rule` added by @MichaReiser on 2023-07-28 06:14_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-07-28 06:14_

---

_@charliermarsh approved on 2023-07-29 00:21_

---

_Merged by @charliermarsh on 2023-07-29 00:34_

---

_Closed by @charliermarsh on 2023-07-29 00:34_

---

_Branch deleted on 2023-07-29 00:35_

---
