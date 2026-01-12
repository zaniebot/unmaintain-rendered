```yaml
number: 3731
title: "[`pydocstyle`] Implement autofix for `D403`"
type: pull_request
state: merged
author: JonathanPlasse
labels:
  - fixes
assignees: []
merged: true
base: main
head: add-d403-auto-fix
created_at: 2023-03-25T14:32:02Z
updated_at: 2023-03-25T19:39:01Z
url: https://github.com/astral-sh/ruff/pull/3731
synced_at: 2026-01-12T04:39:45Z
```

# [`pydocstyle`] Implement autofix for `D403`

---

_Pull request opened by @JonathanPlasse on 2023-03-25 14:32_

- Close #2114

---

_Comment by @github-actions[bot] on 2023-03-25 14:45_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+2, -2, 0 error(s))

<details><summary>bokeh (+1, -1)</summary>
<p>

```diff
- src/bokeh/sphinxext/bokehjs_content.py:129:9: D403 First word of the first line should be properly capitalized
+ src/bokeh/sphinxext/bokehjs_content.py:129:9: D403 [*] First word of the first line should be capitalized: `this` -> `This`
```

</p>
</details>
<details><summary>zulip (+1, -1)</summary>
<p>

```diff
- zerver/lib/markdown/fenced_code.py:539:9: D403 First word of the first line should be properly capitalized
+ zerver/lib/markdown/fenced_code.py:539:9: D403 [*] First word of the first line should be capitalized: `basic` -> `Basic`
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.7±0.08ms     3.0 MB/sec    1.01     13.8±0.11ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.02    498.4±1.13µs     5.9 MB/sec    1.00    488.1±2.14µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.03ms     4.2 MB/sec    1.00      6.0±0.06ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.03ms     5.6 MB/sec    1.00      7.3±0.04ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1648.1±4.74µs    10.1 MB/sec    1.00   1627.7±3.27µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.01    185.5±1.86µs    15.9 MB/sec    1.00    183.1±0.46µs    16.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.4±0.01ms     7.4 MB/sec    1.00      3.4±0.01ms     7.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.1±0.14ms     2.7 MB/sec    1.00     15.1±0.10ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.02ms     4.1 MB/sec    1.00      4.1±0.02ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    453.5±7.97µs     6.5 MB/sec    1.02    461.0±5.11µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.04ms     3.8 MB/sec    1.01      6.8±0.04ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.02ms     5.0 MB/sec    1.01      8.2±0.06ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1752.6±9.60µs     9.5 MB/sec    1.00  1752.2±15.08µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    182.6±1.23µs    16.2 MB/sec    1.01    184.1±3.62µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.02ms     6.7 MB/sec    1.00      3.8±0.02ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Add D403 auto-fix" to "[`pydocstyle`] Implement autofix for `D403`" by @charliermarsh on 2023-03-25 19:15_

---

_Label `autofix` added by @charliermarsh on 2023-03-25 19:16_

---

_Merged by @charliermarsh on 2023-03-25 19:21_

---

_Closed by @charliermarsh on 2023-03-25 19:21_

---

_Branch deleted on 2023-03-25 19:26_

---
