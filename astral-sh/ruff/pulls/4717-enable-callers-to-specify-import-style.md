```yaml
number: 4717
title: "Enable callers to specify import-style preferences in `Importer`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/import-pref
created_at: 2023-05-30T01:35:01Z
updated_at: 2023-05-30T16:58:09Z
url: https://github.com/astral-sh/ruff/pull/4717
synced_at: 2026-01-12T15:55:16Z
```

# Enable callers to specify import-style preferences in `Importer`

---

_@charliermarsh_

## Summary

When we go through the `Importer` flow, to get or import a symbol from an external module during an autofix, if the symbol isn't present in the scope, then we try to add an import to bring it into scope. At present, we always use an `import` statement for this, but sometimes, it might be more conventional to use an `import from`.

For example, when importing symbols from typing, it's more conventional to `from typing import List` than `import typing; typing.List`. This will be especially relevant when importing `TYPE_CHECKING` from `typing` for `flake8-type-checking` autofixes.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-30 01:35_

---

_Review requested from @konstin by @charliermarsh on 2023-05-30 01:35_

---

_Comment by @github-actions[bot] on 2023-05-30 01:51_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.2±0.11ms     2.9 MB/sec    1.00     14.0±0.09ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    427.8±0.95µs     6.9 MB/sec    1.00    426.4±1.00µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.01ms     4.3 MB/sec    1.00      5.9±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.02ms     6.0 MB/sec    1.00      6.8±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1502.2±1.81µs    11.1 MB/sec    1.00   1503.9±9.99µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    171.6±0.30µs    17.2 MB/sec    1.00    170.3±0.68µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.02ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.1±0.01ms     7.9 MB/sec    1.01      5.2±0.00ms     7.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1014.7±0.53µs    16.4 MB/sec    1.01   1020.3±0.84µs    16.3 MB/sec
parser/numpy/globals.py                    1.00    105.1±0.19µs    28.1 MB/sec    1.00    105.6±0.17µs    27.9 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.01ms    11.5 MB/sec    1.00      2.2±0.00ms    11.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     19.9±1.00ms     2.0 MB/sec    1.00     19.8±0.83ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.06      5.3±0.25ms     3.2 MB/sec    1.00      4.9±0.21ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.05   624.6±27.79µs     4.7 MB/sec    1.00   595.3±43.27µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.10      8.9±0.42ms     2.9 MB/sec    1.00      8.1±0.90ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.01      9.4±0.51ms     4.3 MB/sec    1.00      9.3±0.36ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1977.6±79.84µs     8.4 MB/sec    1.05      2.1±0.12ms     8.0 MB/sec
linter/default-rules/numpy/globals.py      1.01   244.2±14.82µs    12.1 MB/sec    1.00   242.8±13.55µs    12.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.20ms     5.9 MB/sec    1.02      4.4±0.28ms     5.8 MB/sec
parser/large/dataset.py                    1.34      9.9±0.44ms     4.1 MB/sec    1.00      7.4±0.40ms     5.5 MB/sec
parser/numpy/ctypeslib.py                  1.06  1603.0±91.04µs    10.4 MB/sec    1.00  1508.6±89.19µs    11.0 MB/sec
parser/numpy/globals.py                    1.20   174.6±10.40µs    16.9 MB/sec    1.00    146.0±7.00µs    20.2 MB/sec
parser/pydantic/types.py                   1.17      3.9±0.15ms     6.5 MB/sec    1.00      3.3±0.17ms     7.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/importer/mod.rs`:154 on 2023-05-30 06:20_

Should this now return a `ImportableSymbol` instead of a `string` or some other enum denoting the used import style? Or is this not possible, because of aliases?

---

_Review comment by @MichaReiser on `crates/ruff/src/importer/mod.rs`:273 on 2023-05-30 06:25_

I like the `API` but find the name `ImportableSymbol` somewhat confusing because two symbols for the same `module` and `member` should compare equal, even if they use different import styles. 

I guess another way to think about it. The qualifying information that defines a symbol is the `module` and the `member`. The style only denotes on how a missing import should be added.

Should we just call this `MemberImport` (or `ImportMember`) or even `Import`?

---

_@MichaReiser approved on 2023-05-30 06:25_

---

_Review comment by @konstin on `crates/ruff/src/importer/mod.rs`:273 on 2023-05-30 15:02_

`ImportRequest` maybe? Since it's less describing a symbol and more "please make an import for this symbol with this preference"

---

_@konstin approved on 2023-05-30 15:06_

---

_Review comment by @charliermarsh on `crates/ruff/src/importer/mod.rs`:154 on 2023-05-30 16:07_

(It's not possible due to aliasing.)

---

_@charliermarsh reviewed on 2023-05-30 16:08_

---

_Merged by @charliermarsh on 2023-05-30 16:46_

---

_Closed by @charliermarsh on 2023-05-30 16:46_

---

_Branch deleted on 2023-05-30 16:46_

---
