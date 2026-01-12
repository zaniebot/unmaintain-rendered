```yaml
number: 3779
title: Improve top-of-file insertions for required imports
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/end-of-docstrings
created_at: 2023-03-28T17:40:32Z
updated_at: 2023-03-29T21:45:48Z
url: https://github.com/astral-sh/ruff/pull/3779
synced_at: 2026-01-12T04:39:45Z
```

# Improve top-of-file insertions for required imports

---

_Pull request opened by @charliermarsh on 2023-03-28 17:40_

## Summary

Small refactor to our "insert at top-of-file" logic, to move more logic into a helper (which we'll reuse elsewhere in the future).

The behavior is also a bit better in some strange cases. For example, given:

```py
"""My docstring"""; x = 1
```

We now modify to:

```py
"""My docstring"""; from __future__ import annotations; x = 1
```

Whereas before, we did:

```py
"""My docstring"""
from __future__ import annotations; x = 1
```

---

_Comment by @github-actions[bot] on 2023-03-28 18:12_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.5±0.05ms     2.8 MB/sec    1.01     14.7±0.04ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.01ms     4.4 MB/sec    1.00      3.8±0.00ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    415.2±3.30µs     7.1 MB/sec    1.00    414.4±1.00µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.02ms     4.0 MB/sec    1.01      6.3±0.01ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.7±0.02ms     5.3 MB/sec    1.01      7.8±0.02ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1718.3±3.82µs     9.7 MB/sec    1.01   1731.2±3.46µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    179.6±0.40µs    16.4 MB/sec    1.02    182.3±1.52µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.1 MB/sec    1.01      3.6±0.01ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.5±0.09ms     2.6 MB/sec    1.00     15.5±0.07ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.02ms     4.0 MB/sec    1.00      4.1±0.03ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    434.4±3.46µs     6.8 MB/sec    1.00    436.2±4.31µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.03ms     3.8 MB/sec    1.00      6.7±0.03ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.02ms     5.0 MB/sec    1.00      8.1±0.02ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1774.0±15.78µs     9.4 MB/sec    1.00   1768.9±8.00µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    187.6±2.58µs    15.7 MB/sec    1.00   187.0±11.85µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.02ms     6.8 MB/sec    1.00      3.7±0.09ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/isort/helpers.rs`:118 on 2023-03-29 06:49_

Nit: `pub(crate)` or make it even private (specific to `isort`). we should avoid making things public because Rust's dead code analysis can only flag non-pub types as unused.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/isort/helpers.rs`:122 on 2023-03-29 06:51_

Nit: I prefer named structures over anonymous tuples because anonymous tuples require me to read the documentation to understand the fields' meaning.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/isort/helpers.rs`:127 on 2023-03-29 06:54_

Feedback outside of this PR's scope: Lexing is expensive (it's probably fine here because we only lex very few tokens). Should we expose the tokens as part of `Locator` or `Stylist` (has a reference to all tokens) or some other struct? 

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/isort/helpers.rs`:127 on 2023-03-29 06:56_

What's the reason for using a `peekable` iterator. We only call `tokens.peek()` once and don't use the iterator any further after. 
```suggestion
        let first_token = lexer::lex_located(locator.skip(location), Mode::Module, location)
            .flatten()
            .next();
```

---

_@MichaReiser approved on 2023-03-29 06:58_

---

_@charliermarsh reviewed on 2023-03-29 21:19_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/isort/helpers.rs`:127 on 2023-03-29 21:19_

One problem here is that this requires _both_ the AST and the token stream, so we can't use the "original" token stream (since it gets consumed when creating the AST).

---

_Merged by @charliermarsh on 2023-03-29 21:25_

---

_Closed by @charliermarsh on 2023-03-29 21:25_

---

_Branch deleted on 2023-03-29 21:25_

---
