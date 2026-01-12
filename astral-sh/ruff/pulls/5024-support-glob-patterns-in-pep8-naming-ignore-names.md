```yaml
number: 5024
title: Support glob patterns in pep8_naming ignore-names
type: pull_request
state: merged
author: Thomasdezeeuw
labels: []
assignees: []
merged: true
base: main
head: thomas/issue-2787
created_at: 2023-06-12T14:32:45Z
updated_at: 2023-06-13T15:37:15Z
url: https://github.com/astral-sh/ruff/pull/5024
synced_at: 2026-01-12T03:43:29Z
```

# Support glob patterns in pep8_naming ignore-names

---

_Pull request opened by @Thomasdezeeuw on 2023-06-12 14:32_

## Summary

 Support glob patterns in pep8_naming ignore-names.

Closes #2787

## Test Plan

No tests added yet, not sure where to add these.

---

_Renamed from "Thomas/issue 2787" to "Support glob patterns in pep8_naming ignore-names" by @Thomasdezeeuw on 2023-06-12 14:36_

---

_Comment by @github-actions[bot] on 2023-06-12 15:04_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      7.5±0.11ms     5.4 MB/sec    1.00      7.4±0.08ms     5.5 MB/sec
formatter/numpy/ctypeslib.py               1.03  1581.2±11.74µs    10.5 MB/sec    1.00  1530.5±16.35µs    10.9 MB/sec
formatter/numpy/globals.py                 1.06    153.2±1.13µs    19.3 MB/sec    1.00    144.3±2.71µs    20.5 MB/sec
formatter/pydantic/types.py                1.02      3.1±0.05ms     8.3 MB/sec    1.00      3.0±0.05ms     8.5 MB/sec
linter/all-rules/large/dataset.py          1.00     16.4±0.15ms     2.5 MB/sec    1.01     16.5±0.15ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.03ms     4.2 MB/sec    1.00      4.0±0.03ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    495.7±1.95µs     6.0 MB/sec    1.00    496.0±4.90µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.08ms     3.7 MB/sec    1.01      6.9±0.11ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.0±0.09ms     5.1 MB/sec    1.00      8.0±0.08ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1729.6±16.50µs     9.6 MB/sec    1.01  1739.4±19.22µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    191.6±1.82µs    15.4 MB/sec    1.00    191.1±3.46µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.0 MB/sec    1.00      3.6±0.04ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.09      8.7±0.05ms     4.7 MB/sec    1.00      8.0±0.11ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.07  1741.8±15.11µs     9.6 MB/sec    1.00  1629.7±11.05µs    10.2 MB/sec
formatter/numpy/globals.py                 1.04    166.6±1.86µs    17.7 MB/sec    1.00    159.7±4.49µs    18.5 MB/sec
formatter/pydantic/types.py                1.08      3.6±0.02ms     7.1 MB/sec    1.00      3.3±0.03ms     7.7 MB/sec
linter/all-rules/large/dataset.py          1.00     17.0±0.10ms     2.4 MB/sec    1.01     17.2±0.16ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.05ms     3.8 MB/sec    1.02      4.5±0.05ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    447.1±7.06µs     6.6 MB/sec    1.01    449.4±7.95µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.5±0.05ms     3.4 MB/sec    1.01      7.6±0.05ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.05ms     4.7 MB/sec    1.01      8.7±0.05ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1781.5±27.92µs     9.3 MB/sec    1.00  1784.9±11.42µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    189.7±2.86µs    15.6 MB/sec    1.01    190.8±2.50µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.02ms     6.6 MB/sec    1.01      3.9±0.02ms     6.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pep8_naming/rules/invalid_argument_name.rs`:6 on 2023-06-12 15:31_

Nit: typo in name (`IdentifierMatcher`)

---

_Review comment by @charliermarsh on `crates/ruff/src/settings/types.rs`:280 on 2023-06-12 15:32_

What do you think about making this `TryFrom`? I worry that falling back to exact-match will feel like "silently failing".

---

_@charliermarsh reviewed on 2023-06-12 15:34_

Looking good, though assume this is a draft due to the pending tests.

---

_Review comment by @Thomasdezeeuw on `crates/ruff/src/rules/pep8_naming/rules/invalid_argument_name.rs`:6 on 2023-06-13 08:35_

This is were autocomplete bites back. Fixed.

---

_Review comment by @Thomasdezeeuw on `crates/ruff/src/settings/types.rs`:280 on 2023-06-13 08:35_

Done.

---

_@Thomasdezeeuw reviewed on 2023-06-13 09:27_

---

_@Thomasdezeeuw reviewed on 2023-06-13 09:31_

---

_Review comment by @Thomasdezeeuw on `crates/ruff/src/rules/pep8_naming/mod.rs`:115 on 2023-06-13 09:31_

Above is the list of rules that currently consider `ignore-names`, to me it seems like some rules are missing, e.g. `InvalidClassName`. Could someone double check this is on purpose?

---

_Marked ready for review by @Thomasdezeeuw on 2023-06-13 09:31_

---

_@charliermarsh reviewed on 2023-06-13 13:23_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pep8_naming/mod.rs`:115 on 2023-06-13 13:23_

This looks like an oversight. Want to address in a separate PR?

---

_@charliermarsh reviewed on 2023-06-13 13:25_

---

_Review comment by @charliermarsh on `crates/ruff/src/settings/mod.rs`:238 on 2023-06-13 13:25_

If you rebase, you'll see we have a similar pattern for `copyright` now:

```rust
copyright: config
    .copyright
    .map(copyright::settings::Settings::try_from)
    .transpose()?
    .unwrap_or_default(),
```

Consider re-using that snippet here, for consistency?

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pep8_naming/settings.rs`:12 on 2023-06-13 13:26_

Nit: can we amend the documentation (`/// A list of names to ignore when considering `pep8-naming` violations.`, below) to mention that globs are supported?

---

_@charliermarsh reviewed on 2023-06-13 13:26_

---

_@charliermarsh approved on 2023-06-13 13:26_

This is great work, nice job.

---

_Review comment by @Thomasdezeeuw on `crates/ruff/src/rules/pep8_naming/mod.rs`:115 on 2023-06-13 14:43_

Created https://github.com/astral-sh/ruff/issues/5050.

---

_@Thomasdezeeuw reviewed on 2023-06-13 14:43_

---

_Merged by @Thomasdezeeuw on 2023-06-13 15:37_

---

_Closed by @Thomasdezeeuw on 2023-06-13 15:37_

---

_Branch deleted on 2023-06-13 15:37_

---
