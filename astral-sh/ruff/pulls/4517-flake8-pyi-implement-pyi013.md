```yaml
number: 4517
title: "[`flake8-pyi`] Implement `PYI013`"
type: pull_request
state: merged
author: density
labels:
  - rule
assignees: []
merged: true
base: main
head: pyi-013
created_at: 2023-05-19T04:24:36Z
updated_at: 2023-05-19T15:57:31Z
url: https://github.com/astral-sh/ruff/pull/4517
synced_at: 2026-01-12T15:55:15Z
```

# [`flake8-pyi`] Implement `PYI013`

---

_@density_

Adds support for Y013 from `flake8-pyi`: Non-empty class body must not contain ...

rel: #848

---

_@MichaReiser approved on 2023-05-19 06:34_

---

_Comment by @charliermarsh on 2023-05-19 12:37_

Thanks! Should have a chance to merge this today.

---

_Marked ready for review by @density on 2023-05-19 12:58_

---

_Comment by @density on 2023-05-19 13:16_

Updated PR to account for a recent refactor of `StmtKind` and friends

---

_Renamed from "`[flake8-pyi]` Implement `PYI013`" to "[`flake8-pyi`] Implement `PYI013`" by @charliermarsh on 2023-05-19 15:29_

---

_Label `rule` added by @charliermarsh on 2023-05-19 15:29_

---

_Merged by @charliermarsh on 2023-05-19 15:39_

---

_Closed by @charliermarsh on 2023-05-19 15:39_

---

_Comment by @github-actions[bot] on 2023-05-19 15:44_

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
linter/all-rules/large/dataset.py          1.02     14.4±0.13ms     2.8 MB/sec    1.00     14.2±0.08ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    422.1±2.14µs     7.0 MB/sec    1.00    423.4±0.56µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.02ms     4.3 MB/sec    1.00      5.9±0.03ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.01ms     6.0 MB/sec    1.00      6.8±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1471.8±3.37µs    11.3 MB/sec    1.00   1464.6±2.74µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.0±0.25µs    18.1 MB/sec    1.02    166.2±7.01µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.00      3.0±0.00ms     8.4 MB/sec
parser/large/dataset.py                    1.01      5.4±0.01ms     7.6 MB/sec    1.00      5.4±0.01ms     7.6 MB/sec
parser/numpy/ctypeslib.py                  1.00   1056.2±0.73µs    15.8 MB/sec    1.00   1054.0±2.76µs    15.8 MB/sec
parser/numpy/globals.py                    1.00    108.6±0.22µs    27.2 MB/sec    1.01    109.2±0.60µs    27.0 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.1 MB/sec    1.00      2.3±0.01ms    11.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.0±0.12ms     2.4 MB/sec    1.00     16.9±0.08ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.02ms     3.8 MB/sec    1.00      4.4±0.03ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    443.4±6.94µs     6.7 MB/sec    1.00    442.9±8.25µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.06ms     3.6 MB/sec    1.00      7.2±0.05ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.04ms     4.8 MB/sec    1.01      8.6±0.03ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1767.8±12.76µs     9.4 MB/sec    1.01  1790.9±19.34µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    191.3±2.41µs    15.4 MB/sec    1.01    193.1±5.91µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.02ms     6.7 MB/sec    1.01      3.9±0.08ms     6.6 MB/sec
parser/large/dataset.py                    1.01      7.7±0.08ms     5.3 MB/sec    1.00      7.6±0.04ms     5.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1425.5±10.82µs    11.7 MB/sec    1.00  1432.0±12.60µs    11.6 MB/sec
parser/numpy/globals.py                    1.00    144.7±1.11µs    20.4 MB/sec    1.01    146.3±1.53µs    20.2 MB/sec
parser/pydantic/types.py                   1.00      3.2±0.02ms     8.0 MB/sec    1.00      3.2±0.03ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Branch deleted on 2023-05-19 15:45_

---
