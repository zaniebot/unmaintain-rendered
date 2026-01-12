```yaml
number: 7662
title: Use 1-based cell indices consistently for Notebooks
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
assignees: []
merged: true
base: main
head: dhruv/consistent-cell-indices
created_at: 2023-09-26T06:25:37Z
updated_at: 2023-09-26T14:14:02Z
url: https://github.com/astral-sh/ruff/pull/7662
synced_at: 2026-01-12T02:39:10Z
```

# Use 1-based cell indices consistently for Notebooks

---

_Pull request opened by @dhruvmanila on 2023-09-26 06:25_

## Summary

This PR fixes the bug where the cell indices displayed in the `--diff` output
and the ones in the normal output were different. This was due to the fact that
the `--diff` output was using the `enumerate` function to iterate over the
cells which starts at 0.

## Test Plan

Ran the following command with and without the `--diff` flag:

```console
cargo run --bin ruff -- check --no-cache --isolated ~/playground/ruff/notebooks/test.ipynb
```

### `main`

<details><summary>Diagnostics output:</summary>
<p>

```console
$ cargo run --bin ruff -- check --no-cache --isolated ~/playground/ruff/notebooks/test.ipynb       
/Users/dhruv/playground/ruff/notebooks/test.ipynb:cell 3:2:8: F401 [*] `math` imported but unused
/Users/dhruv/playground/ruff/notebooks/test.ipynb:cell 5:1:8: F811 Redefinition of unused `random` from line 1
/Users/dhruv/playground/ruff/notebooks/test.ipynb:cell 5:2:8: F401 [*] `pprint` imported but unused
/Users/dhruv/playground/ruff/notebooks/test.ipynb:cell 6:2:4: F632 [*] Use `==` to compare constant literals
/Users/dhruv/playground/ruff/notebooks/test.ipynb:cell 6:3:38: F632 [*] Use `==` to compare constant literals
Found 5 errors.
[*] 4 potentially fixable with the --fix option.
```

</p>
</details>

<details><summary>Diff output:</summary>
<p>

```console
$ cargo run --bin ruff -- check --no-cache --isolated ~/playground/ruff/notebooks/test.ipynb --diff
--- /Users/dhruv/playground/ruff/notebooks/test.ipynb:cell 2
+++ /Users/dhruv/playground/ruff/notebooks/test.ipynb:cell 2
@@ -1,2 +1 @@
-import random
-import math
+import random
--- /Users/dhruv/playground/ruff/notebooks/test.ipynb:cell 4
+++ /Users/dhruv/playground/ruff/notebooks/test.ipynb:cell 4
@@ -1,4 +1,3 @@
 import random
-import pprint
 
 random.randint(10, 20)
--- /Users/dhruv/playground/ruff/notebooks/test.ipynb:cell 5
+++ /Users/dhruv/playground/ruff/notebooks/test.ipynb:cell 5
@@ -1,3 +1,3 @@
 foo = 1
-if foo is 2:
-    raise ValueError(f"Invalid foo: {foo is 1}")
+if foo == 2:
+    raise ValueError(f"Invalid foo: {foo == 1}")

Would fix 4 errors.
```

</p>
</details> 

### `dhruv/consistent-cell-indices`

<details><summary>Diagnostic output:</summary>
<p>

```console
$ cargo run --bin ruff -- check --no-cache --isolated ~/playground/ruff/notebooks/test.ipynb           
/Users/dhruv/playground/ruff/notebooks/test.ipynb:cell 3:2:8: F401 [*] `math` imported but unused
/Users/dhruv/playground/ruff/notebooks/test.ipynb:cell 5:1:8: F811 Redefinition of unused `random` from line 1
/Users/dhruv/playground/ruff/notebooks/test.ipynb:cell 5:2:8: F401 [*] `pprint` imported but unused
/Users/dhruv/playground/ruff/notebooks/test.ipynb:cell 6:2:4: F632 [*] Use `==` to compare constant literals
/Users/dhruv/playground/ruff/notebooks/test.ipynb:cell 6:3:38: F632 [*] Use `==` to compare constant literals
Found 5 errors.
[*] 4 potentially fixable with the --fix option.
```

</p>
</details> 

<details><summary>Diff output:</summary>
<p>

```console
$ cargo run --bin ruff -- check --no-cache --isolated ~/playground/ruff/notebooks/test.ipynb --diff
--- /Users/dhruv/playground/ruff/notebooks/test.ipynb:cell 3
+++ /Users/dhruv/playground/ruff/notebooks/test.ipynb:cell 3
@@ -1,2 +1 @@
-import random
-import math
+import random
--- /Users/dhruv/playground/ruff/notebooks/test.ipynb:cell 5
+++ /Users/dhruv/playground/ruff/notebooks/test.ipynb:cell 5
@@ -1,4 +1,3 @@
 import random
-import pprint
 
 random.randint(10, 20)
--- /Users/dhruv/playground/ruff/notebooks/test.ipynb:cell 6
+++ /Users/dhruv/playground/ruff/notebooks/test.ipynb:cell 6
@@ -1,3 +1,3 @@
 foo = 1
-if foo is 2:
-    raise ValueError(f"Invalid foo: {foo is 1}")
+if foo == 2:
+    raise ValueError(f"Invalid foo: {foo == 1}")

Would fix 4 errors.
```

</p>
</details> 

fixes: #6673


---

_Comment by @dhruvmanila on 2023-09-26 06:25_

Current dependencies on/for this PR:
* main
  * **PR #7662** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7662" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
    * **PR #7663** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7663" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #7664** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7664" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/7662?utm_source=stack-comment).

---

_Label `bug` added by @dhruvmanila on 2023-09-26 06:33_

---

_Comment by @github-actions[bot] on 2023-09-26 06:42_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Review requested from @konstin by @dhruvmanila on 2023-09-26 06:46_

---

_Review requested from @MichaReiser by @dhruvmanila on 2023-09-26 06:46_

---

_@konstin approved on 2023-09-26 09:04_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/diagnostics.rs`:260 on 2023-09-26 11:04_

Should we use a type like `OneIndexed` to make it clear in the API which indices are one or zero indexed? 

---

_@MichaReiser reviewed on 2023-09-26 11:04_

---

_Review comment by @dhruvmanila on `crates/ruff_cli/src/diagnostics.rs`:260 on 2023-09-26 11:24_

Yes, I'm planning on making that refactor for `NotebookIndex` which will be a separate PR.

---

_@dhruvmanila reviewed on 2023-09-26 11:24_

---

_@MichaReiser approved on 2023-09-26 11:36_

---

_Merged by @dhruvmanila on 2023-09-26 14:14_

---

_Closed by @dhruvmanila on 2023-09-26 14:14_

---

_Branch deleted on 2023-09-26 14:14_

---
