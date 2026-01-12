```yaml
number: 4968
title: Allow private accesses within special dunder methods
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/slf
created_at: 2023-06-08T17:29:29Z
updated_at: 2023-06-08T17:54:31Z
url: https://github.com/astral-sh/ruff/pull/4968
synced_at: 2026-01-12T15:55:17Z
```

# Allow private accesses within special dunder methods

---

_@charliermarsh_

## Summary

Allows, e.g., the access on `other` in:

```python
class Foo:
    def __eq__(self, other):
        return self._private_thing == other._private_thing
```

Closes #3933.


---

_Merged by @charliermarsh on 2023-06-08 17:36_

---

_Closed by @charliermarsh on 2023-06-08 17:36_

---

_Branch deleted on 2023-06-08 17:36_

---

_Comment by @github-actions[bot] on 2023-06-08 17:39_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -9, 0 error(s))

<details><summary>airflow (+0, -6)</summary>
<p>

```diff
- airflow/ti_deps/deps/valid_state_dep.py:45:72: SLF001 Private member accessed: `_valid_states`
- airflow/timetables/_cron.py:80:36: SLF001 Private member accessed: `_expression`
- airflow/timetables/_cron.py:80:76: SLF001 Private member accessed: `_timezone`
- airflow/timetables/interval.py:194:31: SLF001 Private member accessed: `_delta`
- airflow/utils/context.py:231:33: SLF001 Private member accessed: `_context`
- airflow/utils/context.py:236:33: SLF001 Private member accessed: `_context`
```

</p>
</details>
<details><summary>bokeh (+0, -3)</summary>
<p>

```diff
- src/bokeh/core/property/bases.py:150:34: SLF001 Private member accessed: `_default`
- src/bokeh/core/property/bases.py:151:31: SLF001 Private member accessed: `_help`
- src/bokeh/core/property/struct.py:76:98: SLF001 Private member accessed: `_optional`
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| SLF001 | 9 | 0 | 9 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.02ms     6.0 MB/sec    1.02      6.9±0.01ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1196.9±2.32µs    13.9 MB/sec    1.02   1224.0±3.15µs    13.6 MB/sec
formatter/numpy/globals.py                 1.00    134.9±5.54µs    21.9 MB/sec    1.00    135.5±0.35µs    21.8 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.01ms     9.3 MB/sec    1.01      2.8±0.02ms     9.1 MB/sec
linter/all-rules/large/dataset.py          1.00     14.8±0.07ms     2.8 MB/sec    1.00     14.7±0.03ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    365.6±1.16µs     8.1 MB/sec    1.00    364.6±5.39µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.03      6.3±0.03ms     4.0 MB/sec    1.00      6.2±0.04ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.02ms     5.6 MB/sec    1.00      7.3±0.02ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1516.3±2.06µs    11.0 MB/sec    1.01   1524.3±4.04µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.3±0.21µs    18.1 MB/sec    1.00    163.5±0.26µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.00ms     7.8 MB/sec    1.00      3.3±0.00ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      9.1±0.30ms     4.5 MB/sec    1.00      8.9±0.28ms     4.6 MB/sec
formatter/numpy/ctypeslib.py               1.00  1545.6±48.21µs    10.8 MB/sec    1.00  1550.3±43.01µs    10.7 MB/sec
formatter/numpy/globals.py                 1.00    171.4±8.16µs    17.2 MB/sec    1.02   175.2±11.09µs    16.8 MB/sec
formatter/pydantic/types.py                1.01      3.7±0.15ms     7.0 MB/sec    1.00      3.6±0.12ms     7.0 MB/sec
linter/all-rules/large/dataset.py          1.04     20.2±0.44ms     2.0 MB/sec    1.00     19.4±0.49ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.05      5.2±0.20ms     3.2 MB/sec    1.00      4.9±0.13ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.01   585.4±20.52µs     5.0 MB/sec    1.00   581.4±18.60µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.03      8.6±0.28ms     3.0 MB/sec    1.00      8.3±0.26ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.02      9.9±0.21ms     4.1 MB/sec    1.00      9.7±0.24ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03      2.1±0.16ms     7.8 MB/sec    1.00      2.1±0.06ms     8.1 MB/sec
linter/default-rules/numpy/globals.py      1.03    237.5±8.47µs    12.4 MB/sec    1.00    231.3±8.92µs    12.8 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.5±0.14ms     5.7 MB/sec    1.00      4.4±0.14ms     5.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
