```yaml
number: 3796
title: "Allow `TID252` to fix all valid module paths"
type: pull_request
state: merged
author: JonathanPlasse
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-tid252-auto-fix
created_at: 2023-03-29T15:26:56Z
updated_at: 2023-03-29T19:50:43Z
url: https://github.com/astral-sh/ruff/pull/3796
synced_at: 2026-01-12T15:55:13Z
```

# Allow `TID252` to fix all valid module paths

---

_@JonathanPlasse_

Allow module with uppercase letters to be auto-fixed.

Issue raised in Discord.

---

_@charliermarsh reviewed on 2023-03-29 15:28_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_tidy_imports/relative_imports.rs`:111 on 2023-03-29 15:28_

Are module names allowed to use keywords? Is that defined in the spec anywhere?

---

_@JonathanPlasse reviewed on 2023-03-29 15:33_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/flake8_tidy_imports/relative_imports.rs`:111 on 2023-03-29 15:33_

I asked ChatGpt :sweat_smile: 
![img](https://live.staticflickr.com/65535/52779854898_ee9913da87_c.jpg)

---

_@JonathanPlasse reviewed on 2023-03-29 15:40_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/flake8_tidy_imports/relative_imports.rs`:111 on 2023-03-29 15:40_

[What are Python Identifiers?](https://www.digitalocean.com/community/tutorials/python-keywords-identifiers#what-are-python-identifiers)
> Python Identifier is the name we give to identify a variable, function, class, **module** or other object. That means whenever we want to give an entity a name, that’s called identifier.
> Sometimes variable and identifier are often misunderstood as same but they are not. Well for clarity, let’s see what is a variable?

---

_Renamed from "Fix TID252 auto-fix" to "Allow `TID252` to fix all valid module paths" by @charliermarsh on 2023-03-29 19:13_

---

_Label `bug` added by @charliermarsh on 2023-03-29 19:13_

---

_Merged by @charliermarsh on 2023-03-29 19:13_

---

_Closed by @charliermarsh on 2023-03-29 19:13_

---

_Comment by @github-actions[bot] on 2023-03-29 19:15_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.7±0.03ms     2.8 MB/sec    1.00     14.7±0.04ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.00ms     4.4 MB/sec    1.00      3.8±0.00ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    415.6±2.10µs     7.1 MB/sec    1.00    414.3±2.54µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.01ms     4.0 MB/sec    1.00      6.4±0.05ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.01ms     5.2 MB/sec    1.00      7.8±0.01ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1722.7±4.32µs     9.7 MB/sec    1.00   1718.0±3.12µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.01    183.0±1.16µs    16.1 MB/sec    1.00    181.3±0.60µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.1 MB/sec    1.00      3.6±0.01ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     16.1±0.15ms     2.5 MB/sec    1.00     15.9±0.10ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.05ms     4.0 MB/sec    1.00      4.1±0.05ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    501.9±6.68µs     5.9 MB/sec    1.00    500.0±8.15µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.8±0.06ms     3.7 MB/sec    1.00      6.8±0.06ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.01      8.3±0.07ms     4.9 MB/sec    1.00      8.3±0.05ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1808.8±14.73µs     9.2 MB/sec    1.01  1819.6±16.73µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.0±4.66µs    15.1 MB/sec    1.00    195.5±6.92µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.8±0.05ms     6.7 MB/sec    1.00      3.8±0.05ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Branch deleted on 2023-03-29 19:50_

---
