```yaml
number: 6825
title: Format all attribute dot comments manually
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/attr
created_at: 2023-08-23T20:57:55Z
updated_at: 2023-08-25T04:15:18Z
url: https://github.com/astral-sh/ruff/pull/6825
synced_at: 2026-01-12T15:55:22Z
```

# Format all attribute dot comments manually

---

_@charliermarsh_

## Summary

This PR modifies our formatting of comments around the `.` in an attribute. Specifically, the goal here is to avoid _reordering_ comments, and the net effect is that we generally leave comments where-they-are when dealing with comments between around the dot (which you can also think of as comments between attributes).

All comments around the dot are now treated as dangling and formatted manually, with the exception of end-of-line or parenthesized comments on the value, like those marked as trailing here, which remain trailing:

```python
(
    (
        a # trailing end-of-line
        # trailing own-line
    ) # dangling before dot end-of-line
    .b # trailing end-of-line
)
```

Closes https://github.com/astral-sh/ruff/issues/6823.

## Test Plan

`cargo test`

Before:

| project      | similarity index |
|--------------|------------------|
| cpython      | 0.76050          |
| django       | 0.99820          |
| transformers | 0.99800          |
| twine        | 0.99876          |
| typeshed     | 0.99953          |
| warehouse    | 0.99615          |
| zulip        | 0.99729          |

After:

| project      | similarity index |
|--------------|------------------|
| cpython      | 0.76050          |
| django       | 0.99820   |
| transformers | 0.99800          |
| twine        | 0.99876          |
| typeshed     | 0.99953          |
| warehouse    | 0.99615          |
| zulip        | 0.99729          |


---

_@charliermarsh reviewed on 2023-08-23 20:58_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/expr_attribute.rs`:187 on 2023-08-23 20:58_

This is the same as the method in placement, but it doesn't have a natural home. Any ideas?

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/expr_attribute.rs`:95 on 2023-08-23 20:59_

We get to unify these branches now which I see as a win.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/expr_attribute.rs`:93 on 2023-08-23 20:59_

No longer necessary, since this is only possible if the node is parenthesized anyway.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/expr_attribute.rs`:166 on 2023-08-23 20:59_

This is much simpler now.

---

_@charliermarsh reviewed on 2023-08-23 20:59_

---

_Comment by @github-actions[bot] on 2023-08-23 21:36_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      4.7±0.07ms     8.6 MB/sec    1.00      4.6±0.06ms     8.8 MB/sec
formatter/numpy/ctypeslib.py               1.01   983.8±10.42µs    16.9 MB/sec    1.00   973.0±12.27µs    17.1 MB/sec
formatter/numpy/globals.py                 1.02     94.3±1.40µs    31.3 MB/sec    1.00     92.4±1.37µs    31.9 MB/sec
formatter/pydantic/types.py                1.01  1933.3±25.17µs    13.2 MB/sec    1.00  1913.8±19.94µs    13.3 MB/sec
linter/all-rules/large/dataset.py          1.01     12.1±0.10ms     3.4 MB/sec    1.00     12.0±0.14ms     3.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.2±0.03ms     5.2 MB/sec    1.00      3.2±0.03ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    455.0±5.60µs     6.5 MB/sec    1.00    454.6±4.97µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.3±0.06ms     4.1 MB/sec    1.00      6.2±0.06ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      6.3±0.05ms     6.4 MB/sec    1.01      6.4±0.05ms     6.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1418.7±13.11µs    11.7 MB/sec    1.00  1419.4±17.06µs    11.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    166.7±1.77µs    17.7 MB/sec    1.01    168.0±1.38µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.02ms     8.8 MB/sec    1.00      2.9±0.03ms     8.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03      6.0±0.27ms     6.8 MB/sec    1.00      5.9±0.21ms     6.9 MB/sec
formatter/numpy/ctypeslib.py               1.02  1248.9±83.22µs    13.3 MB/sec    1.00  1221.9±83.85µs    13.6 MB/sec
formatter/numpy/globals.py                 1.03    114.6±7.56µs    25.7 MB/sec    1.00    111.0±8.45µs    26.6 MB/sec
formatter/pydantic/types.py                1.02      2.5±0.15ms    10.3 MB/sec    1.00      2.4±0.09ms    10.5 MB/sec
linter/all-rules/large/dataset.py          1.00     17.4±0.64ms     2.3 MB/sec    1.00     17.5±0.96ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.5±0.24ms     3.7 MB/sec    1.05      4.8±0.30ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.01   601.1±45.59µs     4.9 MB/sec    1.00   595.1±31.99µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.9±0.37ms     2.9 MB/sec    1.03      9.2±0.36ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00      9.8±0.45ms     4.2 MB/sec    1.03     10.0±0.37ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.10ms     8.1 MB/sec    1.01      2.1±0.09ms     8.0 MB/sec
linter/default-rules/numpy/globals.py      1.07   263.7±19.02µs    11.2 MB/sec    1.00   246.3±15.57µs    12.0 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.5±0.22ms     5.7 MB/sec    1.00      4.4±0.25ms     5.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-23 22:04_

---

_Review requested from @konstin by @charliermarsh on 2023-08-23 22:04_

---

_Label `formatter` added by @charliermarsh on 2023-08-23 22:04_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_attribute.rs`:126 on 2023-08-24 06:55_

Is the writing of the hard line break still necessary after your `dangling_comments` change?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_attribute.rs`:187 on 2023-08-24 06:56_

How about moving it next to `first_non_trivia_token`?

https://github.com/astral-sh/ruff/blob/86ccdcc9d99086b15e86f3d07b83d39a9422069d/crates/ruff_python_trivia/src/tokenizer.rs#L16


---

_@MichaReiser approved on 2023-08-24 06:57_

Thank you so much for handling all these comments with so much care. 

Nit: It would be helpful for reviewers if you could add a small summary above the similarity index table, e.g. no changes, or small improvements for a few projects. It removes the need for myself to scan through the two tables 

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_attribute.rs`:187 on 2023-08-24 10:27_

you could make it a `SimpleTokenizer` method

---

_@konstin approved on 2023-08-24 10:28_

---

_@charliermarsh reviewed on 2023-08-24 18:20_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/expr_attribute.rs`:126 on 2023-08-24 18:20_

No, good eye! But that's downstream of this, it's removed in the next PR.

---

_Merged by @charliermarsh on 2023-08-25 03:50_

---

_Closed by @charliermarsh on 2023-08-25 03:50_

---

_Branch deleted on 2023-08-25 03:50_

---
