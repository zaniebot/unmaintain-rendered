```yaml
number: 4367
title: Expose more fields in rule explanation
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: expose-more-fields-in-rule-explanation
created_at: 2023-05-11T06:38:01Z
updated_at: 2023-05-12T04:21:43Z
url: https://github.com/astral-sh/ruff/pull/4367
synced_at: 2026-01-12T15:55:15Z
```

# Expose more fields in rule explanation

---

_@JonathanPlasse_

- Close #4343

---

_Review comment by @JonathanPlasse on `crates/ruff_cli/src/commands/rule.rs`:16 on 2023-05-11 06:38_

Should summary be deprecated in favor of message_formats?

---

_@JonathanPlasse reviewed on 2023-05-11 06:38_

---

_@JonathanPlasse reviewed on 2023-05-11 06:39_

---

_Review comment by @JonathanPlasse on `crates/ruff_cli/src/commands/rule.rs`:13 on 2023-05-11 06:39_

Should the rule name be exposed?
Are all rule names stabilized?

---

_Comment by @github-actions[bot] on 2023-05-11 06:47_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.7±0.52ms     2.4 MB/sec    1.06     17.7±1.03ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.24ms     4.0 MB/sec    1.02      4.2±0.21ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   508.4±21.46µs     5.8 MB/sec    1.09   552.1±33.30µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.41ms     3.6 MB/sec    1.10      7.7±0.51ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.05      8.5±0.23ms     4.8 MB/sec    1.00      8.1±0.40ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1803.4±84.45µs     9.2 MB/sec    1.00  1784.4±93.30µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    213.7±8.49µs    13.8 MB/sec    1.06   226.3±18.29µs    13.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.17ms     6.6 MB/sec    1.05      4.1±0.24ms     6.3 MB/sec
parser/large/dataset.py                    1.00      6.6±0.22ms     6.1 MB/sec    1.08      7.1±0.29ms     5.7 MB/sec
parser/numpy/ctypeslib.py                  1.00  1298.6±54.01µs    12.8 MB/sec    1.08  1407.3±61.74µs    11.8 MB/sec
parser/numpy/globals.py                    1.00    129.3±8.31µs    22.8 MB/sec    1.04    134.9±9.00µs    21.9 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.17ms     8.9 MB/sec    1.06      3.0±0.20ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.3±0.20ms     2.8 MB/sec    1.00     14.2±0.17ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.04ms     4.5 MB/sec    1.00      3.7±0.05ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    434.6±8.78µs     6.8 MB/sec    1.00    434.9±8.52µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.09ms     4.2 MB/sec    1.00      6.1±0.07ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.07ms     5.7 MB/sec    1.01      7.2±0.08ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1495.5±21.28µs    11.1 MB/sec    1.02  1529.6±21.15µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    167.1±3.95µs    17.7 MB/sec    1.03    172.8±4.16µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.05ms     8.0 MB/sec    1.00      3.2±0.05ms     7.9 MB/sec
parser/large/dataset.py                    1.00      5.9±0.07ms     6.9 MB/sec    1.01      6.0±0.06ms     6.8 MB/sec
parser/numpy/ctypeslib.py                  1.00  1113.8±20.50µs    15.0 MB/sec    1.01  1127.9±26.15µs    14.8 MB/sec
parser/numpy/globals.py                    1.00    113.5±2.59µs    26.0 MB/sec    1.00    113.6±2.65µs    26.0 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.04ms    10.2 MB/sec    1.01      2.5±0.04ms    10.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @charliermarsh by @MichaReiser on 2023-05-11 07:45_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/rule.rs`:13 on 2023-05-11 23:20_

I think this is ok.

---

_@charliermarsh reviewed on 2023-05-11 23:20_

---

_@charliermarsh reviewed on 2023-05-11 23:20_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/rule.rs`:16 on 2023-05-11 23:20_

Probably... I suppose we can leave it for now, I don't know if any clients depend on this.

---

_Merged by @charliermarsh on 2023-05-11 23:22_

---

_Closed by @charliermarsh on 2023-05-11 23:22_

---

_Branch deleted on 2023-05-12 04:21_

---
