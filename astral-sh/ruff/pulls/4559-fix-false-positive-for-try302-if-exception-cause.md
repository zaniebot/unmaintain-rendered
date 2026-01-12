```yaml
number: 4559
title: Fix false-positive for TRY302 if exception cause is given
type: pull_request
state: merged
author: 153957
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-try302
created_at: 2023-05-21T13:06:04Z
updated_at: 2023-05-21T16:07:57Z
url: https://github.com/astral-sh/ruff/pull/4559
synced_at: 2026-01-12T03:50:03Z
```

# Fix false-positive for TRY302 if exception cause is given

---

_Pull request opened by @153957 on 2023-05-21 13:06_

Fix #4548.

Add new test to fixtures with expected success, but it fails with old rule implementation: https://github.com/153957/ruff/actions/runs/5037490516/jobs/9034361352#step:7:1861

And is successful with the change to the rule, as the CI for this PR will show.

---

_Comment by @JonathanPlasse on 2023-05-21 13:11_

Can you add a fixture when the cause is not `None`?

---

_Comment by @github-actions[bot] on 2023-05-21 13:15_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -3, 0 error(s))

<details><summary>zulip (+0, -3)</summary>
<p>

```diff
- scripts/lib/setup_venv.py:344:9: TRY302 Remove exception handler; error is immediately re-raised
- tools/lib/provision.py:388:13: TRY302 Remove exception handler; error is immediately re-raised
- zerver/lib/test_runner.py:222:5: TRY302 Remove exception handler; error is immediately re-raised
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| TRY302 | 3 | 0 | 3 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     17.3±0.15ms     2.4 MB/sec    1.00     16.9±0.08ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.01ms     4.0 MB/sec    1.00      4.1±0.02ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    510.5±2.68µs     5.8 MB/sec    1.00    506.1±2.54µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.1±0.06ms     3.6 MB/sec    1.00      7.0±0.04ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.02      8.2±0.04ms     5.0 MB/sec    1.00      8.0±0.05ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1764.5±8.75µs     9.4 MB/sec    1.00  1729.1±10.16µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.02    195.9±0.94µs    15.1 MB/sec    1.00    192.5±1.86µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.7±0.02ms     6.9 MB/sec    1.00      3.6±0.02ms     7.1 MB/sec
parser/large/dataset.py                    1.01      6.4±0.01ms     6.3 MB/sec    1.00      6.4±0.02ms     6.4 MB/sec
parser/numpy/ctypeslib.py                  1.00   1265.1±3.78µs    13.2 MB/sec    1.00   1261.7±4.96µs    13.2 MB/sec
parser/numpy/globals.py                    1.00    130.8±1.08µs    22.6 MB/sec    1.04    136.0±7.71µs    21.7 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.01ms     9.3 MB/sec    1.00      2.7±0.01ms     9.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.06     17.3±0.13ms     2.3 MB/sec    1.00     16.4±0.09ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      4.4±0.04ms     3.8 MB/sec    1.00      4.2±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.03   508.8±12.61µs     5.8 MB/sec    1.00    495.6±6.73µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.05      7.3±0.08ms     3.5 MB/sec    1.00      6.9±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.11      9.2±0.05ms     4.4 MB/sec    1.00      8.2±0.05ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.09  1880.7±20.19µs     8.9 MB/sec    1.00  1726.4±17.19µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.05    206.3±3.11µs    14.3 MB/sec    1.00    196.1±4.89µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.09      4.0±0.04ms     6.3 MB/sec    1.00      3.7±0.04ms     6.9 MB/sec
parser/large/dataset.py                    1.01      6.8±0.05ms     6.0 MB/sec    1.00      6.7±0.05ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.01  1286.2±16.66µs    12.9 MB/sec    1.00  1271.8±18.75µs    13.1 MB/sec
parser/numpy/globals.py                    1.02    133.4±2.47µs    22.1 MB/sec    1.00    131.1±1.76µs    22.5 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.02ms     8.9 MB/sec    1.00      2.8±0.04ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-05-21 15:49_

---

_Closed by @charliermarsh on 2023-05-21 15:49_

---

_Comment by @charliermarsh on 2023-05-21 15:49_

Thanks :)

---

_Label `bug` added by @charliermarsh on 2023-05-21 15:50_

---

_Branch deleted on 2023-05-21 16:07_

---
