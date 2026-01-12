```yaml
number: 4283
title: "Avoid `D403` if first char cannot be uppercased"
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: main
head: fix/D403/skip-if-no-uppercase
created_at: 2023-05-08T13:44:23Z
updated_at: 2023-05-08T23:11:45Z
url: https://github.com/astral-sh/ruff/pull/4283
synced_at: 2026-01-12T03:56:39Z
```

# Avoid `D403` if first char cannot be uppercased

---

_Pull request opened by @dhruvmanila on 2023-05-08 13:44_

fixes: #4280 

---

_Comment by @github-actions[bot] on 2023-05-08 13:56_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     18.9±0.37ms     2.2 MB/sec    1.01     19.1±0.30ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.5±0.10ms     3.7 MB/sec    1.00      4.4±0.13ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   550.6±18.27µs     5.4 MB/sec    1.01   554.4±16.67µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.9±0.34ms     3.2 MB/sec    1.00      7.8±0.16ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      9.3±0.27ms     4.4 MB/sec    1.02      9.4±0.26ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1954.5±75.94µs     8.5 MB/sec    1.01  1965.4±45.28µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    224.7±8.69µs    13.1 MB/sec    1.02   230.2±12.02µs    12.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.25ms     6.2 MB/sec    1.01      4.2±0.11ms     6.1 MB/sec
parser/large/dataset.py                    1.00      7.4±0.14ms     5.5 MB/sec    1.01      7.5±0.11ms     5.4 MB/sec
parser/numpy/ctypeslib.py                  1.00  1453.4±35.60µs    11.5 MB/sec    1.00  1447.2±27.49µs    11.5 MB/sec
parser/numpy/globals.py                    1.00    143.0±4.53µs    20.6 MB/sec    1.00    142.4±3.90µs    20.7 MB/sec
parser/pydantic/types.py                   1.01      3.2±0.18ms     8.0 MB/sec    1.00      3.2±0.07ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     20.2±0.64ms     2.0 MB/sec    1.00     20.0±0.54ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      5.2±0.24ms     3.2 MB/sec    1.00      5.1±0.21ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.01   606.0±34.53µs     4.9 MB/sec    1.00   602.8±33.23µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.6±0.42ms     3.0 MB/sec    1.00      8.5±0.30ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.04     10.6±0.59ms     3.8 MB/sec    1.00     10.2±0.38ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.11ms     8.0 MB/sec    1.04      2.2±0.07ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   234.4±16.64µs    12.6 MB/sec    1.06   248.7±13.75µs    11.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.16ms     5.9 MB/sec    1.06      4.6±0.17ms     5.6 MB/sec
parser/large/dataset.py                    1.00      8.3±0.21ms     4.9 MB/sec    1.00      8.3±0.24ms     4.9 MB/sec
parser/numpy/ctypeslib.py                  1.01  1604.5±76.99µs    10.4 MB/sec    1.00  1595.2±47.21µs    10.4 MB/sec
parser/numpy/globals.py                    1.01    161.7±9.81µs    18.3 MB/sec    1.00    160.1±6.69µs    18.4 MB/sec
parser/pydantic/types.py                   1.01      3.5±0.11ms     7.2 MB/sec    1.00      3.5±0.13ms     7.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pydocstyle/rules/capitalized.rs`:70 on 2023-05-08 14:12_

Nit: I wonder if we should change the check above to `!first_char.is_lowercase()`  (maybe add a comment why we aren't using `is_uppercase`. I might be wrong but I would expect that every char that is lowercase can be converted to uppercase. 

Edit: 
I think your fix is safer and easier to understand.

---

_@MichaReiser approved on 2023-05-08 14:12_

---

_@charliermarsh reviewed on 2023-05-08 17:47_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pydocstyle/rules/capitalized.rs`:64 on 2023-05-08 17:47_

Do we know why `to_uppercase` returns an iterator? When would it have 0, or more than 1 element?

---

_Merged by @charliermarsh on 2023-05-08 19:33_

---

_Closed by @charliermarsh on 2023-05-08 19:33_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/pydocstyle/rules/capitalized.rs`:64 on 2023-05-08 23:11_

Because Unicode.

The problem is that there's absolutely no guarantee that converting the case of a single code point will result in a single code point. For example, ["ß" uppercases to "SS"](http://unicode.org/faq/casemap_charprop.html#11).

We avoid checking further if it's not an ASCII alphabet whose code is just a few lines above this, but I think you already updated the code :)



---

_@dhruvmanila reviewed on 2023-05-08 23:11_

---

_Branch deleted on 2023-05-08 23:11_

---
