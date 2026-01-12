```yaml
number: 6776
title: Improve formatter contributor docs
type: pull_request
state: merged
author: konstin
labels:
  - documentation
assignees: []
merged: true
base: main
head: improve-ruff-python-formatter-docs
created_at: 2023-08-22T15:52:52Z
updated_at: 2023-08-24T11:14:30Z
url: https://github.com/astral-sh/ruff/pull/6776
synced_at: 2026-01-12T15:55:22Z
```

# Improve formatter contributor docs

---

_@konstin_

The docs were out of date, and the new version incorporates some feedback.

I tried to keep the language concise and the information ordered by how early you need it, so people can get the relevant information quickly before jumping into the code.

I did some minor format_dev changes for consistency in the docs.

---

_Comment by @github-actions[bot] on 2023-08-22 16:26_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.09      3.6±0.04ms    11.3 MB/sec    1.00      3.3±0.04ms    12.3 MB/sec
formatter/numpy/ctypeslib.py               1.10   752.0±12.10µs    22.1 MB/sec    1.00   683.0±10.42µs    24.4 MB/sec
formatter/numpy/globals.py                 1.09     78.2±0.26µs    37.8 MB/sec    1.00     71.5±0.57µs    41.3 MB/sec
formatter/pydantic/types.py                1.15  1540.3±10.16µs    16.6 MB/sec    1.00  1334.7±13.09µs    19.1 MB/sec
linter/all-rules/large/dataset.py          1.00     10.9±0.01ms     3.7 MB/sec    1.02     11.2±0.02ms     3.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.01ms     5.7 MB/sec    1.02      3.0±0.00ms     5.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    326.8±1.80µs     9.0 MB/sec    1.01    328.6±1.09µs     9.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.6±0.01ms     4.6 MB/sec    1.02      5.7±0.01ms     4.5 MB/sec
linter/default-rules/large/dataset.py      1.00      6.0±0.01ms     6.8 MB/sec    1.05      6.3±0.03ms     6.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1245.1±3.62µs    13.4 MB/sec    1.03   1286.7±2.49µs    12.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    128.6±0.39µs    22.9 MB/sec    1.01    129.7±1.74µs    22.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.7±0.01ms     9.6 MB/sec    1.04      2.8±0.01ms     9.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.15      5.1±0.15ms     7.9 MB/sec    1.00      4.4±0.13ms     9.2 MB/sec
formatter/numpy/ctypeslib.py               1.11  1015.6±39.25µs    16.4 MB/sec    1.00   916.0±40.84µs    18.2 MB/sec
formatter/numpy/globals.py                 1.14    106.9±3.48µs    27.6 MB/sec    1.00     93.7±4.53µs    31.5 MB/sec
formatter/pydantic/types.py                1.19      2.2±0.06ms    11.7 MB/sec    1.00  1835.2±69.58µs    13.9 MB/sec
linter/all-rules/large/dataset.py          1.00     14.9±0.34ms     2.7 MB/sec    1.05     15.6±0.63ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.15ms     4.1 MB/sec    1.01      4.1±0.14ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   511.2±20.54µs     5.8 MB/sec    1.02   520.7±19.43µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.7±0.18ms     3.3 MB/sec    1.02      7.9±0.23ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.16ms     4.9 MB/sec    1.01      8.4±0.18ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1818.2±57.68µs     9.2 MB/sec    1.00  1795.3±49.33µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    213.0±7.04µs    13.9 MB/sec    1.00    210.0±7.93µs    14.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.09ms     6.6 MB/sec    1.00      3.8±0.12ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @konstin on 2023-08-23 10:43_

---

_Label `documentation` added by @MichaReiser on 2023-08-24 07:29_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/README.md`:9 on 2023-08-24 07:30_

:rofl: 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/README.md`:75 on 2023-08-24 07:31_

```suggestion
cd playground && npm install && npm run dev:wasm && npm run dev
```


---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/README.md`:135 on 2023-08-24 07:32_

I have to give this a try someday

---

_@MichaReiser approved on 2023-08-24 07:33_

---

_@konstin reviewed on 2023-08-24 10:38_

---

_Review comment by @konstin on `crates/ruff_python_formatter/README.md`:75 on 2023-08-24 10:38_

fwiw the playground readme is still the other way round

---

_Merged by @konstin on 2023-08-24 10:45_

---

_Closed by @konstin on 2023-08-24 10:45_

---

_Branch deleted on 2023-08-24 10:45_

---
