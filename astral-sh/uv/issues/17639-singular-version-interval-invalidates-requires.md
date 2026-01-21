```yaml
number: 17639
title: "Singular version interval invalidates `requires-dist` for `uv sync --no-install-project`"
type: issue
state: open
author: pjonsson
labels:
  - bug
assignees: []
created_at: 2026-01-21T13:57:23Z
updated_at: 2026-01-21T14:19:26Z
url: https://github.com/astral-sh/uv/issues/17639
synced_at: 2026-01-21T15:05:52Z
```

# Singular version interval invalidates `requires-dist` for `uv sync --no-install-project`

---

_@pjonsson_

### Summary

This commit:
```patch
diff --git a/package/pyproject.toml b/package/pyproject.toml
index 9ffe607c..fae72a39 100644
--- a/package/pyproject.toml
+++ b/package/pyproject.toml
@@ -38,7 +38,8 @@ dependencies = [
     "psycopg[c] >= 3.2.1, < 4",
     # FIXME: <ticketnumber>
     "pystac >= 1.7.3, < 1.12",
-    "rasterio >= 1.4.3, < 2",
+    # FIXME: update to 1.5 with GDAL 3.12.2 upgrade.
+    "rasterio >= 1.4.4, <= 1.4.4",
     "shapely >= 2.0.1, < 3",
     "SQLAlchemy >= 2.0.24, < 3",
     "tqdm >= 4.65.0, < 5",
diff --git a/package/uv.lock b/package/uv.lock
index b9719a35..fe1cb635 100644
--- a/package/uv.lock
+++ b/package/uv.lock
@@ -2346,7 +2346,7 @@ requires-dist = [
     { name = "netcdf4", specifier = ">=1.6.3,<2" },
     { name = "psycopg", extras = ["c"], specifier = ">=3.2.1,<4" },
     { name = "pystac", specifier = ">=1.7.3,<1.12" },
-    { name = "rasterio", specifier = ">=1.4.3,<2" },
+    { name = "rasterio", specifier = "<=1.4.4,>=1.4.4" },
     { name = "shapely", specifier = ">=2.0.1,<3" },
     { name = "sqlalchemy", specifier = ">=2.0.24,<3" },
     { name = "tqdm", specifier = ">=4.65.0,<5" },
```
makes verbose mode `uv sync --locked --no-dev --no-install-project --no-binary-package rasterio` go from saying this:
```
DEBUG uv 0.9.21
DEBUG Acquired shared lock for `/root/.cache/uv`
DEBUG Found project root: `/build/package`
DEBUG No workspace root found, using project root
DEBUG Acquired exclusive lock for `/build/package`
DEBUG Using Python request `3.12` from explicit request
DEBUG Checking for Python environment at: `/app`
DEBUG Using request timeout of 30s
DEBUG Searching for Python 3.12 in managed installations or search path
DEBUG Searching for managed installations at `/root/.local/share/uv/python`
DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `/usr/bin/python3.12` (first executable in the search path)
Using CPython 3.12.3 interpreter at: /usr/bin/python3.12
Creating virtual environment at: /app
DEBUG Using base executable for virtual environment: /usr/bin/python3.12
DEBUG Released lock at `/tmp/uv-2858aec583077cb8.lock`
DEBUG Acquired exclusive lock for `/app`
DEBUG Using request timeout of 30s
DEBUG Found static `requires-dist` for: /build/package/
DEBUG No workspace root found, using project root
DEBUG Static `provides-extra` for `package @ editable+.` is up-to-date
DEBUG Static `requires-dist` for `package @ editable+.` is up-to-date
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 133 packages in 1ms
DEBUG Omitting `package` from resolution due to `--no-install-project`
DEBUG Using request timeout of 30s
DEBUG Identified uncached distribution: alembic==1.17.2
DEBUG Identified uncached distribution: boto3==1.42.19
<similar lines for the rest of the dependencies in uv.lock>
```
into saying this:
```
...
DEBUG No workspace root found, using project root
DEBUG Static `provides-extra` for `package @ editable+.` is up-to-date
DEBUG Static `requires-dist` for `package @ editable+.` is out-of-date; falling back to distribution database
DEBUG No static `pyproject.toml` available for: package @ file:///build/package (DynamicField("version"))
DEBUG Acquired exclusive lock for `/root/.cache/uv/sdists-v9/editable/f2d181c3b5a4320a`
DEBUG Computed cache info: Timestamp(SystemTime { tv_sec: 1768833961, tv_nsec: 247762711 }), None, None, {}, {"src": None}. Most recently modified: pyproject.toml
DEBUG Preparing metadata for: package @ file:///build/package
DEBUG Assessing Python executable as base candidate: /usr/bin/python3.12
DEBUG No workspace root found, using project root
DEBUG Using base executable for virtual environment: /usr/bin/python3.12
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.12.3
DEBUG Solving with target Python version: >=3.12.3
DEBUG Adding direct dependency: hatchling*
DEBUG Adding direct dependency: hatch-vcs*
DEBUG No cache entry for: https://pypi.org/simple/hatchling/
```
and then perform dependency resolution. This fails spectacularly since the package is using hatch-vcs and the package sources and `.git` directory will only be copied in after the dependencies have been synced:
```dockerfile
WORKDIR /build

COPY --link package/pyproject.toml /build/package/
COPY --link package/uv.lock /build/package/

RUN cd package && \
    uv sync --locked --no-dev --no-install-project \
        --no-binary-package rasterio

COPY --link /package ./package
COPY --link /.git ./.git
```
Bumping the upper constraint one patch level (no Rasterio 1.4.5 released) gives me this commit:
```patch
-- a/package/pyproject.toml
+++ b/package/pyproject.toml
@@ -39,7 +39,7 @@ dependencies = [
     # FIXME: <ticketnumber>
     "pystac >= 1.7.3, < 1.12",
     # FIXME: update to 1.5 with GDAL 3.12.2 upgrade.
-    "rasterio >= 1.4.4, <= 1.4.4",
+    "rasterio >= 1.4.4, <= 1.4.5",
     "shapely >= 2.0.1, < 3",
     "SQLAlchemy >= 2.0.24, < 3",
     "tqdm >= 4.65.0, < 5",
diff --git a/package/uv.lock b/package/uv.lock
index fe1cb635..4eb04950 100644
--- a/package/uv.lock
+++ b/package/uv.lock
@@ -2346,7 +2346,7 @@ requires-dist = [
     { name = "netcdf4", specifier = ">=1.6.3,<2" },
     { name = "psycopg", extras = ["c"], specifier = ">=3.2.1,<4" },
     { name = "pystac", specifier = ">=1.7.3,<1.12" },
-    { name = "rasterio", specifier = "<=1.4.4,>=1.4.4" },
+    { name = "rasterio", specifier = ">=1.4.4,<=1.4.5" },
     { name = "shapely", specifier = ">=2.0.1,<3" },
     { name = "sqlalchemy", specifier = ">=2.0.24,<3" },
     { name = "tqdm", specifier = ">=4.65.0,<5" },
```
and uv is back to its regular behavior.

Further testing makes it appear that these intervals give a similar diff to `uv.lock` and work just fine:
1. (-oo, 1.4.4]
1. [1.4.4, oo)
1. [1.4.4, 1.4.5)
1. [1.4.4, 1.4.5]
1. (1.4.3, 1.4.4]
1. [1.4.3, 1.4.4]

but for some reason this fails:
1. [1.4.4, 1.4.4]

If I specify `rasterio == 1.4.4`, that also works fine.

The debug logs for this issue say uv 0.9.21, but yesterday I did my initial testing with uv 0.9.26 and that gives me the same behavior. My initial testing also included trying to pass `--no-editable` along with `--no-install-project`, but that did not help.

If `rasterio >= 1.4.4, <= 1.4.4` is not a legal version specifier for some reason, `uv` should give an error message.

### Platform

Ubuntu 24.04 x86_64

### Version

0.9.26 and 0.9.21

### Python version

Python 3.12.3

---

_Label `bug` added by @pjonsson on 2026-01-21 13:57_

---

_Comment by @pjonsson on 2026-01-21 14:19_

I don't know if it's related, but looking at my report, it appears the specifier in `uv.lock` has flipped the order of the upper and lower range for the interval compared to the surrounding lines.

---
