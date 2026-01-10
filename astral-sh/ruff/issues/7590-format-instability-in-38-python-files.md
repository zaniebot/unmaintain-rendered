```yaml
number: 7590
title: Format instability in 38 python files 
type: issue
state: closed
author: qarmin
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-09-22T07:10:01Z
updated_at: 2023-09-22T16:37:39Z
url: https://github.com/astral-sh/ruff/issues/7590
synced_at: 2026-01-10T11:09:49Z
```

# Format instability in 38 python files 

---

_Issue opened by @qarmin on 2023-09-22 07:10_

0.0.290 latest master branch

Zip contains 3 folders:
- BD - original files
- BD2 - files after running first time ruff
- BD3 - files after running second time ruff on original files

and diff13986975832760036465.txt that shows diffs between files after second and first ruff run

[broken_files.zip](https://github.com/astral-sh/ruff/files/12697754/broken_files.zip)

Example instability
```
//////////////////////////////////////////////////////
--- /home/rafal/test/DOWNLOADED/broken_files//BD2/A_16119982587513585207.py	2023-09-22 09:04:47.281983181 +0200
+++ /home/rafal/test/DOWNLOADED/broken_files//BD3/A_16119982587513585207.py	2023-09-22 09:04:47.357981173 +0200
@@ -210,8 +210,7 @@
     input_shapes = [get_shape(v) for v in inputs]
     assert len(input_shapes) == 2, input_shapes
     assert (
-        input_shapes[0][-1]
-        == input_shapes[1][-2]  # type: ignore
+        input_shapes[0][-1] == input_shapes[1][-2]  # type: ignore
     ), input_shapes  # type: ignore
     flop = prod(input_shapes[0]) * input_shapes[-1][-1]  # type: ignore
     return flop
```
```
--- /home/rafal/test/DOWNLOADED/broken_files//BD2/A_7173176388825988664.py	2023-09-22 09:04:47.285983076 +0200
+++ /home/rafal/test/DOWNLOADED/broken_files//BD3/A_7173176388825988664.py	2023-09-22 09:04:47.357981173 +0200
@@ -78,7 +78,7 @@
         *_DISK_META_MAGIC,
         *memoryview(
             (image_name.encode("utf-8") + b"\x00" * _DISK_META_IMAGE_NAME_SIZE)[
-                  # type: ignore
+                # type: ignore
                 :_DISK_META_IMAGE_NAME_SIZE
             ]
         ).cast("c"),
```
```
//////////////////////////////////////////////////////
--- /home/rafal/test/DOWNLOADED/broken_files//BD2/A_14230110707883433080.py	2023-09-22 09:04:47.289982970 +0200
+++ /home/rafal/test/DOWNLOADED/broken_files//BD3/A_14230110707883433080.py	2023-09-22 09:04:47.349981385 +0200
@@ -412,7 +412,6 @@
 
             # else:
             # print(xsrfRequest.headers["x-csrf-token"],xsrfRequest.headers)
-
     except Exception as e:
         raise GetTokenException(
             f"'Get x-csrf-token' process returned an Exception: {e}'"
```
```
--- /home/rafal/test/DOWNLOADED/broken_files//BD2/A_1798643049678049848.py	2023-09-22 09:04:47.277983287 +0200
+++ /home/rafal/test/DOWNLOADED/broken_files//BD3/A_1798643049678049848.py	2023-09-22 09:04:47.349981385 +0200
@@ -212,8 +212,7 @@
     + list(itertools.product([0], ONE_TO_NINE + TEN_TO_LOTS))  # integer value of 0
     + list(itertools.product(ONE_TO_LOTS, [0]))  # decimal value of 0
     + list(itertools.product(NEGATIVE_NUMBERS, ONE_TO_NINE))  # negative integer
-    +
-    list(itertools.product(ONE_TO_LOTS, NEGATIVE_NUMBERS))  # negative decimal
+    + list(itertools.product(ONE_TO_LOTS, NEGATIVE_NUMBERS))  # negative decimal
 ] + [
     str(components[0]) + "." + str(components[1])
     for components in itertools.product(
```
```
--- /home/rafal/test/DOWNLOADED/broken_files//BD2/A_870428921595896654.py	2023-09-22 09:04:47.281983181 +0200
+++ /home/rafal/test/DOWNLOADED/broken_files//BD3/A_870428921595896654.py	2023-09-22 09:04:47.349981385 +0200
@@ -3,15 +3,13 @@
 
 
 def f2(
-    x: e2 = #
-    "foo",
+    x: e2 = "foo",  #
 ) -> r2:
     pass
 
 
 def f3(  #
-    x: e3 = #  # =  #
-    123,  #
+    x: e3 = 123,  #  # =  #  #
     #
 ) -> r3:
     pass
```

---

_Label `bug` added by @charliermarsh on 2023-09-22 13:32_

---

_Label `formatter` added by @charliermarsh on 2023-09-22 13:32_

---

_Comment by @charliermarsh on 2023-09-22 16:37_

Okay, I tried a bunch of these into their own issues -- let's close for now and then rerun at some point once those are resolved. Thank you!

---

_Closed by @charliermarsh on 2023-09-22 16:37_

---
