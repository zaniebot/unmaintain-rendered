```yaml
number: 3835
title: "When checking module visibility, don't check entire ancestry"
type: pull_request
state: merged
author: Hnasar
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-private-parent-bug
created_at: 2023-03-31T20:48:37Z
updated_at: 2023-04-04T22:37:39Z
url: https://github.com/astral-sh/ruff/pull/3835
synced_at: 2026-01-12T04:28:19Z
```

# When checking module visibility, don't check entire ancestry

---

_Pull request opened by @Hnasar on 2023-03-31 20:48_

When checking module visibility, don't check entire ancestry

I have a network system mounted on a directory that starts with an underscore, e.g. /mnt/_foo/home/timothy/ projects/my_package/foo.py`

I found that ruff wasn't showing lints for undocumented public classes, functions or modules in foo.py.

When checking for module visibility, we should stop after the module path. (In ruff, unlike pydocstyle, the module visibility overrides any
class or function visibility.)

This should fix the bug where an unrelated parent directory affects the visibility checks.

---

_Comment by @github-actions[bot] on 2023-03-31 21:00_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+8, -0, 0 error(s))

<details><summary>bokeh (+8, -0)</summary>
<p>

```diff
+ tests/unit/bokeh/_testing/util/test_env.py:1:1: D100 Missing docstring in public module
+ tests/unit/bokeh/_testing/util/test_env.py:45:5: D103 Missing docstring in public function
+ tests/unit/bokeh/_testing/util/test_env.py:51:5: D103 Missing docstring in public function
+ tests/unit/bokeh/_testing/util/test_env.py:66:5: D103 Missing docstring in public function
+ tests/unit/bokeh/_testing/util/test_env.py:73:5: D103 Missing docstring in public function
+ tests/unit/bokeh/_testing/util/test_env.py:80:5: D103 Missing docstring in public function
+ tests/unit/bokeh/_testing/util/test_env.py:87:5: D103 Missing docstring in public function
+ tests/unit/bokeh/_testing/util/test_env.py:94:5: D103 Missing docstring in public function
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     15.1±0.04ms     2.7 MB/sec    1.00     14.7±0.06ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.8±0.00ms     4.4 MB/sec    1.00      3.8±0.01ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    414.4±1.81µs     7.1 MB/sec    1.00    412.4±1.65µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.5±0.04ms     3.9 MB/sec    1.00      6.3±0.01ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.03      8.0±0.05ms     5.1 MB/sec    1.00      7.8±0.02ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1761.0±7.13µs     9.5 MB/sec    1.00   1732.5±7.66µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.02    185.1±1.66µs    15.9 MB/sec    1.00    180.9±0.40µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.7±0.01ms     7.0 MB/sec    1.00      3.6±0.00ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     20.9±0.43ms  1992.7 KB/sec    1.00     20.8±0.41ms  2006.3 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      5.4±0.24ms     3.1 MB/sec    1.00      5.3±0.13ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.02   660.2±31.01µs     4.5 MB/sec    1.00   650.1±31.62µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.02      9.1±0.26ms     2.8 MB/sec    1.00      8.9±0.23ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.02     10.7±0.21ms     3.8 MB/sec    1.00     10.5±0.19ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.3±0.07ms     7.1 MB/sec    1.00      2.3±0.11ms     7.2 MB/sec
linter/default-rules/numpy/globals.py      1.05   278.0±24.35µs    10.6 MB/sec    1.00   265.5±12.32µs    11.1 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.9±0.12ms     5.2 MB/sec    1.00      4.8±0.15ms     5.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-04-01 03:47_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/visibility.rs`:133 on 2023-04-01 03:47_

I think we may be able to look at `module_path` here rather than `Path`:

```rust
/// Create a module path from a (package, path) pair.
///
/// For example, if the package is `foo/bar` and the path is `foo/bar/baz.py`,
/// the call path is `["baz"]`.
pub fn to_module_path(package: &Path, path: &Path) -> Option<Vec<String>> {
  ...
}
```

(The only caller of this already has `module_path` in scope.)

I think that would solve the same problem, but would use our centralized module detection logic rather than implementing additional `__init__.py` checks here. Can you take a look and see if that works?

---

_Label `bug` added by @charliermarsh on 2023-04-01 14:24_

---

_@Hnasar reviewed on 2023-04-02 21:46_

---

_Review comment by @Hnasar on `crates/ruff_python_ast/src/visibility.rs`:133 on 2023-04-02 21:46_

Thanks for the review! I refactored it to take `module_path` reference.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/visibility.rs`:133 on 2023-04-03 07:17_

Nit: 
```suggestion
pub fn module_visibility(module_path: Option<&[String]>, path: &Path) -> Visibility {
```

---

_@MichaReiser reviewed on 2023-04-03 07:17_

---

_Review comment by @Hnasar on `crates/ruff_python_ast/src/visibility.rs`:133 on 2023-04-03 12:35_

Thanks for the review! Changed.

---

_@Hnasar reviewed on 2023-04-03 12:35_

---

_Merged by @charliermarsh on 2023-04-03 15:38_

---

_Closed by @charliermarsh on 2023-04-03 15:38_

---

_Branch deleted on 2023-04-04 22:37_

---
