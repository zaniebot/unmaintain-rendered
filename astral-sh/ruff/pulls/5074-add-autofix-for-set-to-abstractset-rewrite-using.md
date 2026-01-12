```yaml
number: 5074
title: "Add autofix for `Set`-to-`AbstractSet` rewrite using reference tracking"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/rename-symbols
created_at: 2023-06-14T03:47:35Z
updated_at: 2023-06-16T14:25:35Z
url: https://github.com/astral-sh/ruff/pull/5074
synced_at: 2026-01-12T15:55:17Z
```

# Add autofix for `Set`-to-`AbstractSet` rewrite using reference tracking

---

_@charliermarsh_

## Summary

This PR enables autofix behavior for the `flake8-pyi` rule that asks you to alias `Set` to `AbstractSet` when importing `collections.abc.Set`. It's not the most important rule, but it's a good isolated test-case for local symbol renaming.

The renaming algorithm is outlined in-detail in the `renamer.rs` module. But to demonstrate the behavior, here's the diff when running this fix over a complex file that exercises a few edge cases:

```diff
--- a/foo.pyi
+++ b/foo.pyi
@@ -1,16 +1,16 @@
 if True:
-    from collections.abc import Set
+    from collections.abc import Set as AbstractSet
 else:
-    Set = 1
+    AbstractSet = 1

-x: Set = set()
+x: AbstractSet = set()

-x: Set
+x: AbstractSet

-del Set
+del AbstractSet

 def f():
-    print(Set)
+    print(AbstractSet)

     def Set():
         pass
```

Making this work required resolving a bunch of edge cases in the semantic model that were causing us to "lose track" of references. For example, the above wasn't possible with our previous approach to handling deletions (#5071). Similarly, the `x: Set` "delayed annotation" tracking was enabled via #5070. And many of these edits would've failed if we hadn't changed `BindingKind` to always match the identifier range (#5090). So it's really the culmination of a bunch of changes over the course of the week.

The main outstanding TODO is that this doesn't support `global` or `nonlocal` usages. I'm going to take a look at that tonight, but I'm comfortable merging this as-is.

Closes #1106.

Closes #5091.


---

_Comment by @github-actions[bot] on 2023-06-14 03:58_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -5, 0 error(s))

<details><summary>disnake (+0, -5)</summary>
<p>

```diff
- disnake/ext/commands/common_bot_base.py:35:12: TCH004 Move import `importlib.machinery` out of type-checking block. Import is used for more than type hinting.
- tests/ext/commands/test_core.py:9:35: TCH004 Move import `typing_extensions.assert_type` out of type-checking block. Import is used for more than type hinting.
- tests/ui/test_action_row.py:13:35: TCH004 Move import `typing_extensions.assert_type` out of type-checking block. Import is used for more than type hinting.
- tests/ui/test_action_row.py:15:28: TCH004 Move import `disnake.ui.MessageUIComponent` out of type-checking block. Import is used for more than type hinting.
- tests/ui/test_action_row.py:15:48: TCH004 Move import `disnake.ui.ModalUIComponent` out of type-checking block. Import is used for more than type hinting.
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| TCH004 | 5 | 0 | 5 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.9±0.03ms     5.9 MB/sec    1.01      6.9±0.02ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1400.4±4.20µs    11.9 MB/sec    1.00   1406.4±4.59µs    11.8 MB/sec
formatter/numpy/globals.py                 1.00    137.5±0.58µs    21.5 MB/sec    1.00    137.6±1.12µs    21.4 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.01ms     9.1 MB/sec    1.00      2.8±0.01ms     9.1 MB/sec
linter/all-rules/large/dataset.py          1.00     14.1±0.05ms     2.9 MB/sec    1.00     14.2±0.11ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.00ms     4.8 MB/sec    1.00      3.5±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    367.6±1.39µs     8.0 MB/sec    1.00    369.2±0.81µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.03      6.3±0.06ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.03ms     5.6 MB/sec    1.00      7.2±0.03ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1516.6±3.30µs    11.0 MB/sec    1.00   1516.7±4.56µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.7±0.95µs    18.0 MB/sec    1.01    164.6±0.97µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.01ms     7.7 MB/sec    1.00      3.3±0.04ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.0±0.43ms     3.7 MB/sec    1.03     11.4±0.43ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.3±0.10ms     7.3 MB/sec    1.01      2.3±0.11ms     7.2 MB/sec
formatter/numpy/globals.py                 1.00   221.0±13.79µs    13.3 MB/sec    1.06   234.5±25.06µs    12.6 MB/sec
formatter/pydantic/types.py                1.00      4.5±0.19ms     5.7 MB/sec    1.05      4.7±0.27ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     23.7±0.71ms  1757.5 KB/sec    1.02     24.2±0.92ms  1722.0 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      6.1±0.31ms     2.7 MB/sec    1.01      6.1±0.37ms     2.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   702.4±37.79µs     4.2 MB/sec    1.03   722.2±38.36µs     4.1 MB/sec
linter/all-rules/pydantic/types.py         1.01     10.6±0.44ms     2.4 MB/sec    1.00     10.5±0.48ms     2.4 MB/sec
linter/default-rules/large/dataset.py      1.01     12.3±0.76ms     3.3 MB/sec    1.00     12.1±0.39ms     3.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.5±0.10ms     6.6 MB/sec    1.00      2.5±0.10ms     6.7 MB/sec
linter/default-rules/numpy/globals.py      1.03   304.9±24.24µs     9.7 MB/sec    1.00   296.8±18.75µs     9.9 MB/sec
linter/default-rules/pydantic/types.py     1.03      5.5±0.21ms     4.6 MB/sec    1.00      5.4±0.19ms     4.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @konstin by @charliermarsh on 2023-06-15 20:33_

---

_Marked ready for review by @charliermarsh on 2023-06-15 20:33_

---

_@charliermarsh reviewed on 2023-06-15 20:41_

---

_Review comment by @charliermarsh on `crates/ruff/src/renamer.rs`:155 on 2023-06-15 20:41_

This is arguably a bit dangerous. It won't work for select submodules that aren't exported on the parent, like:

```python
>>> import importlib
>>> importlib.util
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: module 'importlib' has no attribute 'util'
```

But, I'm not sure how else to resolve this, and this seems like an edge case for now.

---

_Review comment by @konstin on `crates/ruff/src/renamer.rs`:143 on 2023-06-16 08:06_

do we need a pseudo edit to preserve `import pandas` here, too?

---

_Review comment by @konstin on `crates/ruff/src/renamer.rs`:91 on 2023-06-16 08:08_

That is a cool feature

---

_@konstin approved on 2023-06-16 08:08_

---

_@charliermarsh reviewed on 2023-06-16 14:02_

---

_Review comment by @charliermarsh on `crates/ruff/src/renamer.rs`:143 on 2023-06-16 14:02_

Yes, good call.

---

_Merged by @charliermarsh on 2023-06-16 14:12_

---

_Closed by @charliermarsh on 2023-06-16 14:12_

---

_Branch deleted on 2023-06-16 14:12_

---
