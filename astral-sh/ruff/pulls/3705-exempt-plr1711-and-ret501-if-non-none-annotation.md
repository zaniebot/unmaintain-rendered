```yaml
number: 3705
title: "Exempt `PLR1711` and `RET501` if non-`None` annotation"
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: exempt-annotated-function-return-none
created_at: 2023-03-24T01:34:57Z
updated_at: 2024-07-05T12:14:01Z
url: https://github.com/astral-sh/ruff/pull/3705
synced_at: 2026-01-12T15:55:13Z
```

# Exempt `PLR1711` and `RET501` if non-`None` annotation

---

_@JonathanPlasse_

- Close #3704

---

_Comment by @github-actions[bot] on 2023-03-24 01:49_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -5, 0 error(s))

<details><summary>airflow (+0, -1)</summary>
<p>

```diff
- airflow/secrets/base_secrets.py:152:9: RET501 [*] Do not explicitly `return None` in function if it is the only possible return value
```

</p>
</details>
<details><summary>bokeh (+0, -2)</summary>
<p>

```diff
- src/bokeh/application/handlers/handler.py:232:9: RET501 [*] Do not explicitly `return None` in function if it is the only possible return value
- src/bokeh/util/compiler.py:464:5: RET501 [*] Do not explicitly `return None` in function if it is the only possible return value
```

</p>
</details>
<details><summary>zulip (+0, -2)</summary>
<p>

```diff
- zerver/lib/test_helpers.py:122:9: PLR1711 [*] Useless `return` statement at end of function
- zerver/lib/test_helpers.py:122:9: RET501 [*] Do not explicitly `return None` in function if it is the only possible return value
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     14.5±0.07ms     2.8 MB/sec    1.00     14.2±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.8±0.03ms     4.3 MB/sec    1.00      3.8±0.03ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.01    437.8±1.87µs     6.7 MB/sec    1.00    431.4±1.62µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.03      6.4±0.04ms     4.0 MB/sec    1.00      6.3±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.9±0.08ms     5.2 MB/sec    1.00      7.8±0.06ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1719.3±4.57µs     9.7 MB/sec    1.00   1689.7±9.64µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    174.3±0.50µs    16.9 MB/sec    1.00    173.6±1.39µs    17.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.01ms     7.0 MB/sec    1.00      3.6±0.01ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     21.9±0.57ms  1905.5 KB/sec    1.04     22.8±0.60ms  1825.5 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.7±0.25ms     2.9 MB/sec    1.03      5.9±0.25ms     2.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   719.8±40.44µs     4.1 MB/sec    1.00   722.1±36.41µs     4.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.6±0.42ms     2.6 MB/sec    1.04     10.0±0.37ms     2.5 MB/sec
linter/default-rules/large/dataset.py      1.00     11.4±0.40ms     3.6 MB/sec    1.10     12.6±0.39ms     3.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.5±0.15ms     6.7 MB/sec    1.05      2.6±0.10ms     6.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   290.5±14.76µs    10.2 MB/sec    1.01   294.6±19.57µs    10.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.2±0.18ms     4.9 MB/sec    1.09      5.6±0.26ms     4.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Exempt PLR1711 and RET501 if non None annotation" to "Exempt `PLR1711` and `RET501` if non-`None` annotation" by @charliermarsh on 2023-03-24 03:06_

---

_Merged by @charliermarsh on 2023-03-24 03:11_

---

_Closed by @charliermarsh on 2023-03-24 03:11_

---

_Branch deleted on 2023-03-24 03:15_

---

_Comment by @epenet on 2024-07-05 12:14_

Note: in case of inheritance, it would be great if this exemption could also look at the signature of the parent methods
See #12198 

---
