```yaml
number: 1065
title: Migrate invalid_literal_comparisons fix to token-based logic
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/F632
created_at: 2022-12-05T16:13:20Z
updated_at: 2022-12-05T16:17:01Z
url: https://github.com/astral-sh/ruff/pull/1065
synced_at: 2026-01-12T15:55:05Z
```

# Migrate invalid_literal_comparisons fix to token-based logic

---

_@charliermarsh_

Resolves #1057.

---

_Comment by @charliermarsh on 2022-12-05 16:16_

Tested this on Dagster:

```
diff --git a/examples/docs_snippets/docs_snippets/concepts/types/object_type.py b/examples/docs_snippets/docs_snippets/concepts/types/object_type.py
index 95fb1104fd..e7360eed16 100644
--- a/examples/docs_snippets/docs_snippets/concepts/types/object_type.py
+++ b/examples/docs_snippets/docs_snippets/concepts/types/object_type.py
@@ -4,7 +4,7 @@
 # start_object_type
 class EvenType:
     def __init__(self, num):
-        assert num % 2 is 0
+        assert num % 2 == 0
         self.num = num
 
 
diff --git a/examples/docs_snippets/docs_snippets_tests/intro_tutorial_tests/test_type_guide.py b/examples/docs_snippets/docs_snippets_tests/intro_tutorial_tests/test_type_guide.py
index 651d859e64..5503a161f6 100755
--- a/examples/docs_snippets/docs_snippets_tests/intro_tutorial_tests/test_type_guide.py
+++ b/examples/docs_snippets/docs_snippets_tests/intro_tutorial_tests/test_type_guide.py
@@ -29,7 +29,7 @@ def test_basic_even_type():
     # start_test_basic_even_type
     EvenDagsterType = DagsterType(
         name="EvenDagsterType",
-        type_check_fn=lambda _, value: isinstance(value, int) and value % 2 is 0,
+        type_check_fn=lambda _, value: isinstance(value, int) and value % 2 == 0,
     )
     # end_test_basic_even_type
 
@@ -55,7 +55,7 @@ def double_even(num: EvenDagsterType) -> EvenDagsterType:
 def test_basic_even_type_no_annotations():
     EvenDagsterType = DagsterType(
         name="EvenDagsterType",
-        type_check_fn=lambda _, value: isinstance(value, int) and value % 2 is 0,
+        type_check_fn=lambda _, value: isinstance(value, int) and value % 2 == 0,
     )
 
     # start_test_basic_even_type_no_annotations
@@ -82,7 +82,7 @@ def test_python_object_dagster_type():
     # start_object_type
     class EvenType:
         def __init__(self, num):
-            assert num % 2 is 0
+            assert num % 2 == 0
             self.num = num
 
     EvenDagsterType = PythonObjectDagsterType(EvenType, name="EvenDagsterType")
@@ -106,7 +106,7 @@ def test_even_type_loader():
     # start_type_loader
     class EvenType:
         def __init__(self, num):
-            assert num % 2 is 0
+            assert num % 2 == 0
             self.num = num
 
     @dagster_type_loader(int)
@@ -145,7 +145,7 @@ def double_even(even_num: EvenDagsterType) -> EvenDagsterType:
 def test_even_type_materialization_config():
     class EvenType:
         def __init__(self, num):
-            assert num % 2 is 0
+            assert num % 2 == 0
             self.num = num
 
     @dagster_type_materializer({"path": str})
@@ -186,7 +186,7 @@ def test_mypy_compliance():
     # start_mypy
     class EvenType:
         def __init__(self, num):
-            assert num % 2 is 0
+            assert num % 2 == 0
             self.num = num
 
     if typing.TYPE_CHECKING:
@@ -294,7 +294,7 @@ def test_usable_as_dagster_type():
     @usable_as_dagster_type
     class EvenType:
         def __init__(self, num):
-            assert num % 2 is 0
+            assert num % 2 == 0
             self.num = num
 
     # end_usable_as
@@ -309,7 +309,7 @@ def test_make_usable_as_dagster_type():
     # start_make_usable
     class EvenType:
         def __init__(self, num):
-            assert num % 2 is 0
+            assert num % 2 == 0
             self.num = num
 
     EvenDagsterType = PythonObjectDagsterType(
```

---

_Merged by @charliermarsh on 2022-12-05 16:16_

---

_Closed by @charliermarsh on 2022-12-05 16:16_

---

_Branch deleted on 2022-12-05 16:17_

---
