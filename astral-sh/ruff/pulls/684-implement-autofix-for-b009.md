```yaml
number: 684
title: Implement autofix for B009
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: fix-B009
created_at: 2022-11-11T13:37:46Z
updated_at: 2022-11-11T16:09:42Z
url: https://github.com/astral-sh/ruff/pull/684
synced_at: 2026-01-12T15:55:05Z
```

# Implement autofix for B009

---

_@harupy_

```
cargo run -- --no-cache --fix --select B009 resources/test/fixtures/B009_B010.py
```

```diff
diff --git a/resources/test/fixtures/B009_B010.py b/resources/test/fixtures/B009_B010.py
index e002b0f..4c588ff 100644
--- a/resources/test/fixtures/B009_B010.py
+++ b/resources/test/fixtures/B009_B010.py
@@ -14,9 +14,9 @@ getattr(foo, "123abc")
 getattr(foo, "except")
 
 # Invalid usage
-getattr(foo, "bar")
-getattr(foo, "_123abc")
-getattr(foo, "abc123")
+foo.bar
+foo._123abc
+foo.abc123
 
 # Valid setattr usage
 setattr(foo, bar, None)
@@ -42,6 +42,6 @@ class FakeCookieStore:
 
 
 # getattr is still flagged within lambda though
-c = lambda x: getattr(x, "some_attr")
+c = lambda x: x.some_attr
 # should be replaced with
 c = lambda x: x.some_attr
```

---

_@charliermarsh reviewed on 2022-11-11 15:15_

---

_Review comment by @charliermarsh on `src/flake8_bugbear/plugins/getattr_with_constant.rs`:26 on 2022-11-11 15:15_

What happens if we have, like:

```py
getattr(x, r"y")
```

(That is, if the string has a kind or prefix code?)

---

_@charliermarsh reviewed on 2022-11-11 15:43_

---

_Review comment by @charliermarsh on `src/flake8_bugbear/plugins/getattr_with_constant.rs`:26 on 2022-11-11 15:43_

Actually, think I understand this. Will add some tests and merge.

---

_Merged by @charliermarsh on 2022-11-11 16:06_

---

_Closed by @charliermarsh on 2022-11-11 16:06_

---

_@harupy reviewed on 2022-11-11 16:09_

---

_Review comment by @harupy on `src/flake8_bugbear/plugins/getattr_with_constant.rs`:26 on 2022-11-11 16:09_

Thanks for the update!

---
