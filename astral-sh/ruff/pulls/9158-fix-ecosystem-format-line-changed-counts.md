```yaml
number: 9158
title: Fix ecosystem format line changed counts
type: pull_request
state: merged
author: zanieb
labels:
  - ci
assignees: []
merged: true
base: main
head: zb/ecosystem-counts
created_at: 2023-12-16T03:46:14Z
updated_at: 2023-12-16T06:14:18Z
url: https://github.com/astral-sh/ruff/pull/9158
synced_at: 2026-01-12T15:55:27Z
```

# Fix ecosystem format line changed counts

---

_@zanieb_

We were erroneously including patch headers so for each _file_ changed we could include an extra added and removed line e.g. we counted the diff in the following as +4 -4 instead of +3 -3.

```diff
diff --git a/tests/test_param_include_in_schema.py b/tests/test_param_include_in_schema.py
index 26201e9..f461947 100644
--- a/tests/test_param_include_in_schema.py
+++ b/tests/test_param_include_in_schema.py
@@ -9,14 +9,14 @@ app = FastAPI()
 
 @app.get("/hidden_cookie")
 async def hidden_cookie(
-    hidden_cookie: Optional[str] = Cookie(default=None, include_in_schema=False)
+    hidden_cookie: Optional[str] = Cookie(default=None, include_in_schema=False),
 ):
     return {"hidden_cookie": hidden_cookie}
 
 
 @app.get("/hidden_header")
 async def hidden_header(
-    hidden_header: Optional[str] = Header(default=None, include_in_schema=False)
+    hidden_header: Optional[str] = Header(default=None, include_in_schema=False),
 ):
     return {"hidden_header": hidden_header}
 
@@ -28,7 +28,7 @@ async def hidden_path(hidden_path: str = Path(include_in_schema=False)):
 
 @app.get("/hidden_query")
 async def hidden_query(
-    hidden_query: Optional[str] = Query(default=None, include_in_schema=False)
+    hidden_query: Optional[str] = Query(default=None, include_in_schema=False),
 ):
     return {"hidden_query": hidden_query}
 
```

Tested with a single project locally e.g.

> ℹ️ ecosystem check **detected format changes**. (+65 -65 lines in 39 files in 1 projects)
> 
> <details><summary><a href="https://github.com/tiangolo/fastapi">tiangolo/fastapi</a> (+65 -65 lines across 39 files)

instead of

> ℹ️ ecosystem check **detected format changes**. (+104 -104 lines in 39 files in 1 projects)
>
> <details><summary><a href="https://github.com/tiangolo/fastapi">tiangolo/fastapi</a> (+104 -104 lines across 39 files)


---

_Label `ci` added by @zanieb on 2023-12-16 03:46_

---

_Comment by @zanieb on 2023-12-16 03:55_

cc @T-256 thanks for raising!

---

_Comment by @github-actions[bot] on 2023-12-16 04:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@dhruvmanila approved on 2023-12-16 04:41_

---

_Merged by @zanieb on 2023-12-16 06:14_

---

_Closed by @zanieb on 2023-12-16 06:14_

---

_Branch deleted on 2023-12-16 06:14_

---
