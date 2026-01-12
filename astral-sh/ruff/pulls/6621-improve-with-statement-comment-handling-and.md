```yaml
number: 6621
title: "Improve `with` statement comment handling and expression breaking"
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/with
created_at: 2023-08-16T16:34:52Z
updated_at: 2023-08-18T03:45:29Z
url: https://github.com/astral-sh/ruff/pull/6621
synced_at: 2026-01-12T15:55:22Z
```

# Improve `with` statement comment handling and expression breaking

---

_@charliermarsh_

## Summary

The motivating code here was:

```python
with test as (
    # test
foo):
    pass
```

Which we were formatting as:

```python
with test as
# test
(foo):
    pass
```

`with` statements are oddly difficult. This PR makes a bunch of subtle modifications and adds a more extensive test suite. For example, we now only preserve parentheses if there's more than one `WithItem` _or_ a trailing comma; before, we always preserved.

Our formatting is_not_ the same as Black, but here's a diff of our formatted code vs. Black's for the `with.py` test suite. The primary difference is that we tend to break parentheses when they contain comments rather than move them to the end of the life (this is a consistent difference that we make across the codebase):

```diff
diff --git a/crates/ruff_python_formatter/foo.py b/crates/ruff_python_formatter/foo.py
index 85e761080..31625c876 100644
--- a/crates/ruff_python_formatter/foo.py
+++ b/crates/ruff_python_formatter/foo.py
@@ -1,6 +1,4 @@
-with (
-    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
-), aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa:
+with aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa, aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa:
     ...
     # trailing
 
@@ -16,28 +14,33 @@ with (
     # trailing
 
 
-with a, b:  # a  # comma  # c  # colon
+with (
+    a,  # a  # comma
+    b,  # c
+):  # colon
     ...
 
 
 with (
-    a as  # a  # as
-    # own line
-    b,  # b  # comma
+    a as (  # a  # as
+        # own line
+        b
+    ),  # b  # comma
     c,  # c
 ):  # colon
     ...  # body
     # body trailing own
 
-with (
-    a as  # a  # as
+with a as (  # a  # as
     # own line
-    bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb  # b
-):
+    bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
+):  # b
     pass
 
 
-with (a,):  # magic trailing comma
+with (
+    a,
+):  # magic trailing comma
     ...
 
 
@@ -47,6 +50,7 @@ with a:  # should remove brackets
 with aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb as c:
     ...
 
+
 with (
     # leading comment
     a
@@ -74,8 +78,7 @@ with (
 with (
     a  # trailing same line comment
     # trailing own line comment
-    as b
-):
+) as b:
     ...
 
 with (
@@ -87,7 +90,9 @@ with (
 with (
     a
     # trailing own line comment
-) as b:  # trailing as same line comment  # trailing b same line comment
+) as (  # trailing as same line comment
+    b
+):  # trailing b same line comment
     ...
 
 with (
@@ -124,18 +129,24 @@ with (  # comment
     ...
 
 with (  # outer comment
-    CtxManager1() as example1,  # inner comment
+    (  # inner comment
+        CtxManager1()
+    ) as example1,
     CtxManager2() as example2,
     CtxManager3() as example3,
 ):
     ...
 
-with CtxManager() as example:  # outer comment
+with (  # outer comment
+    CtxManager()
+) as example:
     ...
 
 with (  # outer comment
     CtxManager()
-) as example, CtxManager2() as example2:  # inner comment
+) as example, (  # inner comment
+    CtxManager2()
+) as example2:
     ...
 
 with (  # outer comment
@@ -145,7 +156,9 @@ with (  # outer comment
     ...
 
 with (  # outer comment
-    (CtxManager1()),  # inner comment
+    (  # inner comment
+        CtxManager1()
+    ),
     CtxManager2(),
 ) as example:
     ...
@@ -179,7 +192,9 @@ with (
 ):
     pass
 
-with a as (b):  # foo
+with a as (  # foo
+    b
+):
     pass
 
 with f(
@@ -209,17 +224,13 @@ with f(
 ) as b, c as d:
     pass
 
-with (
-    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
-) as b:
+with aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb as b:
     pass
 
 with aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb as b:
     pass
 
-with (
-    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
-) as b, c as d:
+with aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb as b, c as d:
     pass
 
 with (
@@ -230,6 +241,8 @@ with (
     pass
 
 with (
-    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
-) as b, c as d:
+    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
+    + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb as b,
+    c as d,
+):
     pass
```

Closes https://github.com/astral-sh/ruff/issues/6600.
## Test Plan

Before:

| project      | similarity index |
|--------------|------------------|
| cpython      | 0.75473          |
| django       | 0.99804          |
| transformers | 0.99618          |
| twine        | 0.99876          |
| typeshed     | 0.74292          |
| warehouse    | 0.99601          |
| zulip        | 0.99727          |

After:

| project      | similarity index |
|--------------|------------------|
| cpython      | 0.75473          |
| django       | 0.99804          |
| transformers | 0.99618          |
| twine        | 0.99876          |
| typeshed     | 0.74292          |
| warehouse    | 0.99601          |
| zulip        | 0.99727          |

`cargo test`


---

_Comment by @charliermarsh on 2023-08-16 16:38_

I suspect this might _decrease_ compatibility, I'm not sure. It's possible.

---

_Comment by @charliermarsh on 2023-08-16 16:41_

Nevermind, compatibility is unchanged.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-16 16:42_

---

_Marked ready for review by @charliermarsh on 2023-08-16 16:42_

---

_Comment by @charliermarsh on 2023-08-16 16:49_

In a separate PR, I'm going to work to make this comment trailing on `b`:

```python
with (
    a
    as
    b  # leading comment
): ...
```

Right now, it's trailing on the `WithItem`.

---

_Comment by @github-actions[bot] on 2023-08-16 17:09_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.4±0.02ms    12.0 MB/sec    1.00      3.4±0.01ms    11.9 MB/sec
formatter/numpy/ctypeslib.py               1.00    713.5±3.87µs    23.3 MB/sec    1.00    712.8±2.23µs    23.4 MB/sec
formatter/numpy/globals.py                 1.00     76.3±0.33µs    38.6 MB/sec    1.00     76.5±1.00µs    38.6 MB/sec
formatter/pydantic/types.py                1.00  1380.5±12.43µs    18.5 MB/sec    1.01  1395.5±31.70µs    18.3 MB/sec
linter/all-rules/large/dataset.py          1.00     10.2±0.05ms     4.0 MB/sec    1.00     10.3±0.07ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.01ms     6.1 MB/sec    1.01      2.8±0.02ms     6.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    389.0±0.81µs     7.6 MB/sec    1.00    388.2±0.61µs     7.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.3±0.04ms     4.8 MB/sec    1.00      5.3±0.02ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.00      5.4±0.02ms     7.5 MB/sec    1.00      5.4±0.01ms     7.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1203.1±2.16µs    13.8 MB/sec    1.00   1205.5±3.80µs    13.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    140.0±0.76µs    21.1 MB/sec    1.00    140.5±0.27µs    21.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.01ms    10.4 MB/sec    1.00      2.4±0.01ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.8±0.05ms    10.7 MB/sec    1.00      3.8±0.05ms    10.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   769.6±12.05µs    21.6 MB/sec    1.00   773.4±12.83µs    21.5 MB/sec
formatter/numpy/globals.py                 1.00     78.4±1.10µs    37.7 MB/sec    1.02     79.9±2.81µs    37.0 MB/sec
formatter/pydantic/types.py                1.00  1547.5±29.06µs    16.5 MB/sec    1.01  1560.2±27.85µs    16.3 MB/sec
linter/all-rules/large/dataset.py          1.00     12.6±0.08ms     3.2 MB/sec    1.02     12.8±0.13ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.05ms     4.8 MB/sec    1.01      3.5±0.03ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    439.8±5.19µs     6.7 MB/sec    1.02    448.3±6.50µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.12ms     3.8 MB/sec    1.02      6.8±0.12ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.06ms     5.8 MB/sec    1.05      7.3±0.12ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1505.3±22.41µs    11.1 MB/sec    1.02  1540.9±25.46µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    175.4±3.01µs    16.8 MB/sec    1.01    176.4±2.94µs    16.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.03ms     8.1 MB/sec    1.02      3.2±0.05ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `formatter` added by @charliermarsh on 2023-08-16 17:37_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1134 on 2023-08-17 06:03_

And you claim to be unopinionated about formatting :P

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/with_item.rs`:37 on 2023-08-17 06:04_

Nit for consistency (because you don't use `write` in the else branch). I don't feel too opinionated about this. I would also feel fine to always use `write`, because it makes code changes easier

```suggestion
                optional_vars.format().fmt(f)?;
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_with.rs`:154 on 2023-08-17 06:09_

This must respect the `f.options().magic_trailing_comma` option. It could be worth adding an extra test file that sets the option to ignore similar to `skip_magic_trailing_comma.py`

Could we reuse/extract the logic from 

https://github.com/astral-sh/ruff/blob/b1c4c7be69d796186e0a75e0e88955b489343d9a/crates/ruff_python_formatter/src/builders.rs#L197-L216

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_with.rs`:135 on 2023-08-17 06:10_

`are_with_items_parenthesized` seems to return early if there are less than two items. Should we remove the `with.items.len()` check here?

---

_@MichaReiser approved on 2023-08-17 06:10_

---

_@charliermarsh reviewed on 2023-08-18 03:03_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1134 on 2023-08-18 03:03_

I deserve this

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/other/with_item.rs`:37 on 2023-08-18 03:04_

Good call RE consistency. I find myself preferring `write` for the reason you mention.

---

_@charliermarsh reviewed on 2023-08-18 03:04_

---

_@charliermarsh reviewed on 2023-08-18 03:18_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/stmt_with.rs`:154 on 2023-08-18 03:18_

Good call.

---

_Merged by @charliermarsh on 2023-08-18 03:30_

---

_Closed by @charliermarsh on 2023-08-18 03:30_

---

_Branch deleted on 2023-08-18 03:30_

---
