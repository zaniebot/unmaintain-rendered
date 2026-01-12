```yaml
number: 4271
title: PYI015 covers parts of PYI052 
type: pull_request
state: closed
author: sladyn98
labels: []
assignees: []
base: main
head: Y015-combine
created_at: 2023-05-08T00:09:36Z
updated_at: 2023-05-08T18:50:30Z
url: https://github.com/astral-sh/ruff/pull/4271
synced_at: 2026-01-12T15:55:15Z
```

# PYI015 covers parts of PYI052 

---

_@sladyn98_

This PR attempts to combine the functionality of PY105 and PY1015 into one function according to pylint

Fixes #4227 

---

_Comment by @sladyn98 on 2023-05-08 00:10_

@mccullocht I tried combining the functionality of both of them into one function like pylint, does this look like an approach we would like to follow ? 

---

_Comment by @github-actions[bot] on 2023-05-08 00:43_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.07     18.4±0.20ms     2.2 MB/sec    1.00     17.2±0.21ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.05      4.3±0.06ms     3.9 MB/sec    1.00      4.1±0.06ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.02   531.3±13.61µs     5.6 MB/sec    1.00   520.5±11.42µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.06      7.6±0.09ms     3.3 MB/sec    1.00      7.2±0.14ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.14      9.5±0.14ms     4.3 MB/sec    1.00      8.3±0.10ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.10  1967.9±39.25µs     8.5 MB/sec    1.00  1790.9±35.14µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.05    222.8±5.29µs    13.2 MB/sec    1.00    212.6±6.01µs    13.9 MB/sec
linter/default-rules/pydantic/types.py     1.10      4.1±0.08ms     6.1 MB/sec    1.00      3.8±0.06ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.7±0.07ms     6.1 MB/sec    1.00      6.7±0.07ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1317.3±20.27µs    12.6 MB/sec    1.00  1312.8±18.16µs    12.7 MB/sec
parser/numpy/globals.py                    1.00    130.6±5.96µs    22.6 MB/sec    1.00    130.5±3.94µs    22.6 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.08ms     8.8 MB/sec    1.00      2.9±0.07ms     8.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     19.7±0.70ms     2.1 MB/sec    1.01     19.9±0.68ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.9±0.28ms     3.4 MB/sec    1.02      5.0±0.29ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   586.5±28.51µs     5.0 MB/sec    1.00   588.4±36.91µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.4±0.44ms     3.0 MB/sec    1.01      8.4±0.49ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.01      9.9±0.37ms     4.1 MB/sec    1.00      9.8±0.28ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03      2.1±0.17ms     7.8 MB/sec    1.00      2.1±0.10ms     8.0 MB/sec
linter/default-rules/numpy/globals.py      1.00   232.4±12.21µs    12.7 MB/sec    1.03   238.7±16.28µs    12.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.19ms     5.7 MB/sec    1.01      4.5±0.21ms     5.7 MB/sec
parser/large/dataset.py                    1.02      8.2±0.29ms     5.0 MB/sec    1.00      8.1±0.28ms     5.0 MB/sec
parser/numpy/ctypeslib.py                  1.03  1572.8±85.54µs    10.6 MB/sec    1.00  1526.5±56.66µs    10.9 MB/sec
parser/numpy/globals.py                    1.01    163.2±8.82µs    18.1 MB/sec    1.00   160.9±10.38µs    18.3 MB/sec
parser/pydantic/types.py                   1.03      3.6±0.14ms     7.2 MB/sec    1.00      3.4±0.15ms     7.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:1931 on 2023-05-08 06:14_

```suggestion
                                    self, target.as_deref(), value, None,
```

Does `.as_deref()` or `.as_ref()` work?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_pyi/rules/simple_defaults.rs`:419 on 2023-05-08 06:15_

Nit
```suggestion
    if let Some(anno) = annotation {
        if checker.ctx.match_typing_expr(anno, "TypeAlias") {
            return;
        }
        if is_valid_default_value_with_annotation(value, checker, true) {
            return;
        }
    } else if is_valid_default_value_without_annotaiton(value) {
        return;
    }
```

---

_@MichaReiser approved on 2023-05-08 06:15_

---

_Comment by @charliermarsh on 2023-05-08 18:23_

I think there may be a misunderstanding here. The linked issue is mentioning that violations that should be flagged under `PYI052` (not yet implemented) are currently being flagged under `PYI015`. Resolving the issue requires that we implement `PYI052`, and then ensure that cases flagged by `PYI052` are _not_ flagged by `PYI015`.

(`PYI015` was intentionally split into two functions, and I'd prefer to keep them that way for simplicity.)

---

_Closed by @charliermarsh on 2023-05-08 18:23_

---

_Comment by @charliermarsh on 2023-05-08 18:24_

You can see [here](https://github.com/PyCQA/flake8-pyi/blob/7425435c81d852a3c5a4be45a5b714785cec4ce1/pyi.py#L996) in `flake8-pyi` that there's a check for the `Y052` condition, and `Y015` is only triggered in an `else`. That's what we're missing :)

---

_Comment by @sladyn98 on 2023-05-08 18:50_

@charliermarsh Got it will open a new PR
@MichaReiser Thanks for the review though !


---
