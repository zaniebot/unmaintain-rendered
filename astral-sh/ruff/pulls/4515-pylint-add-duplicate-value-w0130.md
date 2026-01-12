```yaml
number: 4515
title: "[`pylint`] Add `duplicate-value` (`W0130`)"
type: pull_request
state: merged
author: hoel-bagard
labels:
  - rule
assignees: []
merged: true
base: main
head: main
created_at: 2023-05-19T03:30:27Z
updated_at: 2023-05-19T15:21:39Z
url: https://github.com/astral-sh/ruff/pull/4515
synced_at: 2026-01-12T15:55:15Z
```

# [`pylint`] Add `duplicate-value` (`W0130`)

---

_@hoel-bagard_

This is part of https://github.com/charliermarsh/ruff/issues/970.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/duplicate_values.rs`:34 on 2023-05-19 03:40_

Nit: `seen_values.insert` will return a boolean that indicates whether the item already exists in the `HashSet`. So you can both check for duplicates and update the set in a single operation.

---

_@charliermarsh reviewed on 2023-05-19 03:40_

---

_@charliermarsh reviewed on 2023-05-19 03:41_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/duplicate_values.rs`:31 on 2023-05-19 03:41_

Does the Pylint rule include duplicate names? Like `{a, a}`?

---

_@hoel-bagard reviewed on 2023-05-19 03:52_

---

_Review comment by @hoel-bagard on `crates/ruff/src/rules/pylint/rules/duplicate_values.rs`:31 on 2023-05-19 03:52_

I tried with
```python
A = 1
my_set = {A, A}
```
and pylint did not generate a warning.

---

_Comment by @github-actions[bot] on 2023-05-19 04:22_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -0, 1 error(s))

<details><summary>bokeh (error)</summary>
https://github.com/bokeh/bokeh ref branch-3.2 select ALL ignore  exclude 
<p>

```
error: TOML parse error at line 169, column 12
    |
169 | [tool.ruff.pyupgrade]
    |            ^^^^^^^^^
unknown field `pyupgrade`, expected one of `allowed-confusables`, `builtins`, `cache-dir`, `dummy-variable-rgx`, `exclude`, `extend`, `extend-exclude`, `extend-include`, `extend-ignore`, `extend-select`, `extend-fixable`, `extend-unfixable`, `external`, `fix`, `fix-only`, `fixable`, `format`, `force-exclude`, `ignore`, `ignore-init-module-imports`, `include`, `line-length`, `required-version`, `respect-gitignore`, `select`, `show-source`, `show-fixes`, `src`, `namespace-packages`, `target-version`, `task-tags`, `typing-modules`, `unfixable`, `flake8-annotations`, `flake8-bandit`, `flake8-bugbear`, `flake8-builtins`, `flake8-comprehensions`, `flake8-errmsg`, `flake8-quotes`, `flake8-self`, `flake8-tidy-imports`, `flake8-type-checking`, `flake8-gettext`, `flake8-implicit-str-concat`, `flake8-import-conventions`, `flake8-pytest-style`, `flake8-unused-arguments`, `isort`, `mccabe`, `pep8-naming`, `pycodestyle`, `pydocstyle`, `pylint`, `per-file-ignores`


```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     15.2±0.04ms     2.7 MB/sec    1.00     15.0±0.03ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.7±0.01ms     4.5 MB/sec    1.00      3.7±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    376.7±1.61µs     7.8 MB/sec    1.00    375.1±1.35µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.3±0.02ms     4.1 MB/sec    1.00      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.02      7.6±0.02ms     5.4 MB/sec    1.00      7.4±0.01ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1568.4±11.51µs    10.6 MB/sec    1.00   1542.3±3.96µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.01    167.6±0.37µs    17.6 MB/sec    1.00    165.9±0.99µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.4±0.01ms     7.6 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
parser/large/dataset.py                    1.01      6.0±0.01ms     6.7 MB/sec    1.00      6.0±0.01ms     6.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1178.9±6.82µs    14.1 MB/sec    1.00  1173.4±22.42µs    14.2 MB/sec
parser/numpy/globals.py                    1.00    122.0±2.12µs    24.2 MB/sec    1.00    122.3±0.43µs    24.1 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.0 MB/sec    1.00      2.5±0.00ms    10.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.8±0.08ms     2.4 MB/sec    1.00     16.7±0.11ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.03ms     3.8 MB/sec    1.00      4.3±0.04ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    438.9±4.84µs     6.7 MB/sec    1.00    440.5±7.72µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.03ms     3.6 MB/sec    1.00      7.1±0.04ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.03ms     4.8 MB/sec    1.00      8.5±0.03ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1749.0±17.55µs     9.5 MB/sec    1.00   1754.8±8.51µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    189.6±1.39µs    15.6 MB/sec    1.01    190.7±2.73µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.01ms     6.7 MB/sec    1.00      3.8±0.02ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.9±0.02ms     5.9 MB/sec    1.02      7.0±0.02ms     5.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1312.2±9.30µs    12.7 MB/sec    1.01   1324.1±7.61µs    12.6 MB/sec
parser/numpy/globals.py                    1.00    136.9±1.34µs    21.5 MB/sec    1.01    138.3±1.87µs    21.3 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.02ms     8.8 MB/sec    1.01      2.9±0.01ms     8.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/duplicate_values.rs`:28 on 2023-05-19 06:38_

I think using `new` is the better choice here to avoid allocating in the case where none of the `elts` is a `constant` expression.

---

_@MichaReiser approved on 2023-05-19 06:38_

---

_@hoel-bagard reviewed on 2023-05-19 07:08_

---

_Review comment by @hoel-bagard on `crates/ruff/src/rules/pylint/rules/duplicate_values.rs`:28 on 2023-05-19 07:08_

@MichaReiser Thank you for the comment.
Do you mean switching to a normal `HashSet`, or using `default` instead of `with_capacity_and_hasher` ?

I can do
```rust
let mut seen_values: HashSet<ComparableExpr> = HashSet::new();
```
Or 
```rust
let mut seen_values: FxHashSet<ComparableExpr> = FxHashSet::default();
```
but it seems like I can't use `new` with the `FxHashSet`.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/duplicate_values.rs`:28 on 2023-05-19 07:17_

Hmm interesting, it must be because of some type constraint. `FxHashSet::default` should be fine.

---

_@MichaReiser reviewed on 2023-05-19 07:17_

---

_Renamed from "[pylint] Add duplicate-value / W0130 rule" to "[`pylint`] Add `duplicate-value` (`W0130`)" by @charliermarsh on 2023-05-19 14:55_

---

_Label `rule` added by @charliermarsh on 2023-05-19 14:56_

---

_Merged by @charliermarsh on 2023-05-19 15:03_

---

_Closed by @charliermarsh on 2023-05-19 15:03_

---
