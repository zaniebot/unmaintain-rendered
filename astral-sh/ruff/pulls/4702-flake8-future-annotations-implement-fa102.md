```yaml
number: 4702
title: "[`flake8-future-annotations`] Implement `FA102`"
type: pull_request
state: merged
author: akx
labels:
  - rule
assignees: []
merged: true
base: main
head: fa102
created_at: 2023-05-29T08:45:26Z
updated_at: 2023-05-29T23:01:54Z
url: https://github.com/astral-sh/ruff/pull/4702
synced_at: 2026-01-12T03:50:03Z
```

# [`flake8-future-annotations`] Implement `FA102`

---

_Pull request opened by @akx on 2023-05-29 08:45_

## Summary

As discussed in #4553, this adds FA102 to complement FA100 (see #3072, #3797):

> **FA102** Missing import when code uses simplified types (list, dict, set, etc)

This is only enabled for the target versions where the simplified annotations aren't valid:

* PEP585 `list[...], dict[...]` are complained about when targeting Python < 3.9
* PEP604 `|` unions are complained about when targeting Python < 3.10

## Test Plan

So far: A couple of new tests (utilizing the same fixtures as the FA100 tests).

Let me know if more is needed...

## TODO

I'm not quite sure what would be the proper way to add the autofix for this, since it'd mean two Diagnostics, if I used `add_required_import`..?

---

_Marked ready for review by @akx on 2023-05-29 08:55_

---

_Comment by @github-actions[bot] on 2023-05-29 09:01_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.2±0.53ms     2.9 MB/sec    1.04     14.9±0.65ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.6±0.15ms     4.6 MB/sec    1.00      3.6±0.17ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   443.6±22.85µs     6.7 MB/sec    1.01   448.7±22.88µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.07      6.3±0.31ms     4.1 MB/sec    1.00      5.8±0.21ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.30ms     5.9 MB/sec    1.02      7.0±0.27ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.10  1696.3±52.89µs     9.8 MB/sec    1.00  1545.4±69.32µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.02    183.7±8.40µs    16.1 MB/sec    1.00    180.9±8.14µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.04      3.4±0.12ms     7.6 MB/sec    1.00      3.2±0.17ms     7.9 MB/sec
parser/large/dataset.py                    1.00      5.3±0.21ms     7.7 MB/sec    1.06      5.6±0.20ms     7.2 MB/sec
parser/numpy/ctypeslib.py                  1.00  1045.9±50.48µs    15.9 MB/sec    1.02  1067.1±70.81µs    15.6 MB/sec
parser/numpy/globals.py                    1.00    109.8±5.37µs    26.9 MB/sec    1.00    109.2±6.69µs    27.0 MB/sec
parser/pydantic/types.py                   1.05      2.3±0.11ms    11.0 MB/sec    1.00      2.2±0.09ms    11.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     17.0±0.11ms     2.4 MB/sec    1.00     16.5±0.10ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.05ms     3.9 MB/sec    1.00      4.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    502.5±7.22µs     5.9 MB/sec    1.00    499.7±9.19µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.1±0.09ms     3.6 MB/sec    1.00      7.0±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.3±0.13ms     4.9 MB/sec    1.00      8.2±0.06ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1760.5±15.49µs     9.5 MB/sec    1.00  1761.2±30.61µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    205.2±3.82µs    14.4 MB/sec    1.01    206.7±5.37µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     6.8 MB/sec    1.00      3.7±0.04ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.4±0.05ms     6.3 MB/sec    1.01      6.5±0.05ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1224.8±12.68µs    13.6 MB/sec    1.01  1231.3±13.92µs    13.5 MB/sec
parser/numpy/globals.py                    1.00    125.1±3.77µs    23.6 MB/sec    1.00    124.6±1.54µs    23.7 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.2 MB/sec    1.00      2.7±0.03ms     9.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Add rule FA102" to "[`flake8-future-annotations`] Implement `FA102`" by @charliermarsh on 2023-05-29 22:28_

---

_Label `rule` added by @charliermarsh on 2023-05-29 22:28_

---

_Merged by @charliermarsh on 2023-05-29 22:41_

---

_Closed by @charliermarsh on 2023-05-29 22:41_

---

_Comment by @charliermarsh on 2023-05-29 22:53_

Thanks :)

---
