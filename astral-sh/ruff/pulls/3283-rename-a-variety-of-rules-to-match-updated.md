```yaml
number: 3283
title: Rename a variety of rules to match updated conventions
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/rename-rules
created_at: 2023-02-28T21:30:47Z
updated_at: 2023-03-18T21:40:37Z
url: https://github.com/astral-sh/ruff/pull/3283
synced_at: 2026-01-12T15:55:12Z
```

# Rename a variety of rules to match updated conventions

---

_@charliermarsh_

See: #2902.

---

_Comment by @charliermarsh on 2023-02-28 21:31_

This will fail, as I've only renamed them in `registry.rs`, for the purpose of review / discussion.

---

_Comment by @charliermarsh on 2023-02-28 21:31_

\cc @not-my-profile -- not urgent, but any feedback on these?

---

_@charliermarsh reviewed on 2023-02-28 21:32_

---

_Review comment by @charliermarsh on `crates/ruff/src/registry.rs`:479 on 2023-02-28 21:32_

Some of these are very long unfortunately.

---

_@not-my-profile reviewed on 2023-03-01 17:11_

---

_Review comment by @not-my-profile on `crates/ruff/src/registry.rs`:270 on 2023-03-01 17:11_

I think the `Reference` can also be stripped from these names.

---

_Review comment by @not-my-profile on `crates/ruff/src/registry.rs`:279 on 2023-03-01 17:12_

An if else block isn't really a manual ternary operator.

---

_@not-my-profile reviewed on 2023-03-01 17:12_

---

_Review comment by @charliermarsh on `crates/ruff/src/registry.rs`:279 on 2023-03-01 17:13_

Any ideas for alternate framings?

---

_@charliermarsh reviewed on 2023-03-01 17:13_

---

_Review comment by @not-my-profile on `crates/ruff/src/registry.rs`:286 on 2023-03-01 17:13_

Maybe just `InDictKeys`?

---

_@not-my-profile reviewed on 2023-03-01 17:13_

---

_@not-my-profile reviewed on 2023-03-01 17:14_

---

_Review comment by @not-my-profile on `crates/ruff/src/registry.rs`:298 on 2023-03-01 17:14_

An if else block isn't really a "manual dict get".

---

_Review comment by @not-my-profile on `crates/ruff/src/registry.rs`:325 on 2023-03-01 17:17_

Maybe `YieldInForLoop`?

---

_@not-my-profile reviewed on 2023-03-01 17:17_

---

_@not-my-profile reviewed on 2023-03-01 17:23_

---

_Review comment by @not-my-profile on `crates/ruff/src/registry.rs`:479 on 2023-03-01 17:23_

I think `PytestFixtureIncorrectParenthesesStyle` would be slightly better (though not shorter). I don't consider the length to be a problem. We have autocompletion in the VS code extension as well as autocompletion for the CLI.

Sidenote: We don't support the [`pytest-fixture-no-parentheses` config setting](https://github.com/m-burst/flake8-pytest-style/blob/v1.7.2/docs/rules/PT001.md), do we?

---

_Review comment by @not-my-profile on `crates/ruff/src/registry.rs`:325 on 2023-03-01 17:28_

hmm ... maybe rather `IterateAndYield`

---

_@not-my-profile reviewed on 2023-03-01 17:28_

---

_@not-my-profile reviewed on 2023-03-01 17:33_

---

_Review comment by @not-my-profile on `crates/ruff/src/registry.rs`:279 on 2023-03-01 17:33_

I guess my main issue with the current name is that Python doesn't even have a ternary operator. An if expression is very similar but it's strictly speaking not a ternary operator.

---

_@not-my-profile reviewed on 2023-03-01 17:37_

---

_Review comment by @not-my-profile on `crates/ruff/src/registry.rs`:279 on 2023-03-01 17:37_

Maybe `IfElseBlockInsteadOfExpr`.

---

_@not-my-profile reviewed on 2023-03-01 17:38_

---

_Review comment by @not-my-profile on `crates/ruff/src/registry.rs`:298 on 2023-03-01 17:38_

And likewise this could be `IfElseBlockInsteadOfDictGet`.

---

_Review comment by @not-my-profile on `crates/ruff/src/registry.rs`:325 on 2023-03-02 05:51_

I think either `YieldInForLoop` or probably rather `IterateAndYield`. Loop is too generic (since it doesn't apply to while loops).

---

_@not-my-profile reviewed on 2023-03-02 05:51_

---

_Comment by @github-actions[bot] on 2023-03-18 21:25_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.0±0.24ms     2.7 MB/sec    1.02     15.3±0.27ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.0±0.05ms     4.2 MB/sec    1.00      3.9±0.09ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.01    452.9±3.10µs     6.5 MB/sec    1.00    448.3±1.49µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.08      7.0±0.10ms     3.7 MB/sec    1.00      6.5±0.01ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.03      8.5±0.01ms     4.8 MB/sec    1.00      8.2±0.02ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1835.1±10.94µs     9.1 MB/sec    1.00   1776.9±6.39µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.01    188.5±0.80µs    15.7 MB/sec    1.00    186.9±0.32µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.07      4.1±0.07ms     6.3 MB/sec    1.00      3.8±0.05ms     6.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.5±0.13ms     2.6 MB/sec    1.01     15.6±0.20ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.06ms     4.0 MB/sec    1.00      4.2±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    536.1±6.59µs     5.5 MB/sec    1.01    540.4±8.11µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.05ms     3.7 MB/sec    1.00      6.8±0.09ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.7±0.08ms     4.7 MB/sec    1.00      8.6±0.07ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1898.8±27.01µs     8.8 MB/sec    1.00  1890.6±29.41µs     8.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    209.5±3.65µs    14.1 MB/sec    1.01    211.3±5.43µs    14.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.0±0.07ms     6.3 MB/sec    1.00      4.0±0.05ms     6.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-03-18 21:36_

---

_Closed by @charliermarsh on 2023-03-18 21:36_

---

_Branch deleted on 2023-03-18 21:36_

---
