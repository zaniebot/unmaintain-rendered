```yaml
number: 4154
title: "[`pylint`] Implement import-self (`W0406`)"
type: pull_request
state: merged
author: chanman3388
labels: []
assignees: []
merged: true
base: main
head: PLW0406
created_at: 2023-04-30T03:48:14Z
updated_at: 2023-05-04T20:16:04Z
url: https://github.com/astral-sh/ruff/pull/4154
synced_at: 2026-01-12T04:28:19Z
```

# [`pylint`] Implement import-self (`W0406`)

---

_Pull request opened by @chanman3388 on 2023-04-30 03:48_

Implement pylint's [`W0406`](https://pylint.pycqa.org/en/latest/user_guide/messages/warning/import-self.html)

Ideally I'd also want an integration test which tests a package too. I have checked that on this simple example that it emits the same diagnostics that pylint does.

```
a
├── __init__.py
├── a
│   └── __init__.py
└── a.py
```
`a.a` contains
```
from . import a
import a.a
from a.a import b
from a.a.a import b


def b():
    pass
```
`a.a.a` is empty.
Ruff:
```
/home/chris/projects/pylint_exp/W0604/a/a.py:1:1: PLW0406 import-self
  |
1 | from . import a
  | ^^^^^^^^^^^^^^^ PLW0406
2 | import a.a
3 | from a.a import b
  |

/home/chris/projects/pylint_exp/W0604/a/a.py:2:1: PLW0406 import-self
  |
2 | from . import a
3 | import a.a
  | ^^^^^^^^^^ PLW0406
4 | from a.a import b
5 | from a.a.a import b
  |

/home/chris/projects/pylint_exp/W0604/a/a.py:3:1: PLW0406 import-self
  |
3 | from . import a
4 | import a.a
5 | from a.a import b
  | ^^^^^^^^^^^^^^^^^ PLW0406
6 | from a.a.a import b
  |

Found 3 errors.
```
Pylint:
```
a/a.py:1:0: W0406: Module import itself (import-self)
a/a.py:2:0: W0406: Module import itself (import-self)
a/a.py:3:0: W0406: Module import itself (import-self)
```

---

_Comment by @github-actions[bot] on 2023-04-30 21:54_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.09     16.7±0.10ms     2.4 MB/sec    1.00     15.2±0.34ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.09      4.0±0.02ms     4.2 MB/sec    1.00      3.7±0.07ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.09    494.0±6.20µs     6.0 MB/sec    1.00   452.1±16.64µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.10      7.1±0.11ms     3.6 MB/sec    1.00      6.4±0.19ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.10      8.0±0.41ms     5.1 MB/sec    1.00      7.3±0.15ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1604.0±66.20µs    10.4 MB/sec    1.00  1608.8±44.36µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    183.0±5.74µs    16.1 MB/sec    1.00    182.7±6.00µs    16.1 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.4±0.08ms     7.6 MB/sec    1.00      3.3±0.08ms     7.8 MB/sec
parser/large/dataset.py                    1.02      6.0±0.16ms     6.8 MB/sec    1.00      5.9±0.13ms     6.9 MB/sec
parser/numpy/ctypeslib.py                  1.08  1267.3±11.07µs    13.1 MB/sec    1.00  1169.0±31.84µs    14.2 MB/sec
parser/numpy/globals.py                    1.08    129.2±0.36µs    22.8 MB/sec    1.00    119.8±3.47µs    24.6 MB/sec
parser/pydantic/types.py                   1.09      2.8±0.00ms     9.2 MB/sec    1.00      2.5±0.07ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.1±0.13ms     2.4 MB/sec    1.00     16.9±0.12ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.03ms     3.9 MB/sec    1.00      4.3±0.03ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    437.8±7.15µs     6.7 MB/sec    1.00    439.2±7.31µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.07ms     3.5 MB/sec    1.01      7.2±0.10ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.05ms     4.7 MB/sec    1.00      8.6±0.07ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1816.6±17.43µs     9.2 MB/sec    1.00  1820.4±11.26µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    194.2±2.01µs    15.2 MB/sec    1.00    191.8±2.90µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.02ms     6.6 MB/sec    1.00      3.9±0.02ms     6.6 MB/sec
parser/large/dataset.py                    1.01      6.9±0.03ms     5.9 MB/sec    1.00      6.8±0.03ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1297.9±10.53µs    12.8 MB/sec    1.00   1292.5±9.67µs    12.9 MB/sec
parser/numpy/globals.py                    1.00    133.3±1.13µs    22.1 MB/sec    1.00    133.0±1.38µs    22.2 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.02ms     8.8 MB/sec    1.00      2.9±0.02ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/import_self.rs`:53 on 2023-05-01 10:23_

Nit:
```suggestion
                    if level == Some(1) {
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/import_self.rs`:73 on 2023-05-01 10:26_

```suggestion
    module_path: Option<&[String]>,
```

And use `.as_deref()` on the callsite

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/import_self.rs`:35 on 2023-05-01 10:27_

Nit: We should move the `.join` call out of the loop to avoid allocating a new `String` over every name
```suggestion
				let joined_module_path = module_path.join(".");
                names
                    .iter()
                    .any(|name| name.node.name == joined_module_path)
```

Or we can use `split` on `name.node.name` to avoid allocating a new string entirely: 

```rust
  names
                    .iter()
                    .any(|name| name.node.name.split('.').eq(module_path.iter()))
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/import_self.rs`:41 on 2023-05-01 10:33_

We can bail early if `get_module_from_path` returns `None` without having to check all names
```suggestion
			} else if let Some(module) = get_module_from_path(path) {
                names.iter().any(|name| name.node.name == module)
            } else {
                false
            }
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/import_self.rs`:51 on 2023-05-01 10:36_

Nit: Use `split` and `Iterator::eq`?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/import_self.rs`:56 on 2023-05-01 10:37_

Nit: Bail early if `module_path.last()` is `None`

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/snapshots/ruff__rules__pylint__tests__PLW0406_import_self.py.snap`:4 on 2023-05-01 10:37_

```suggestion
import_self.py:1:1: PLW0406 Module imports itself
```

---

_@MichaReiser approved on 2023-05-01 10:37_

---

_Comment by @MichaReiser on 2023-05-01 10:37_

This is looking good. I've a few smaller nit improvements that may help with performance

---

_@chanman3388 reviewed on 2023-05-01 21:09_

---

_Review comment by @chanman3388 on `crates/ruff/src/rules/pylint/snapshots/ruff__rules__pylint__tests__PLW0406_import_self.py.snap`:4 on 2023-05-01 21:09_

I would favour this too, but this is the message that pylint emits, should we mirror their behaviour? I don't mind if we don't, just got to do what makes sense.

---

_@charliermarsh reviewed on 2023-05-02 01:10_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/snapshots/ruff__rules__pylint__tests__PLW0406_import_self.py.snap`:4 on 2023-05-02 01:10_

We often deviate from upstream when it comes to the messages we omit, if we think we can make them clearer.

---

_Comment by @chanman3388 on 2023-05-02 01:32_

Thanks for all the comments, I believe I'll made all the requested adjustments.

---

_@MichaReiser approved on 2023-05-02 07:07_

---

_Merged by @charliermarsh on 2023-05-04 20:05_

---

_Closed by @charliermarsh on 2023-05-04 20:05_

---
