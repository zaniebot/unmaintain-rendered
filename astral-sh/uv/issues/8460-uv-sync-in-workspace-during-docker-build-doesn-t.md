```yaml
number: 8460
title: "`uv` sync in workspace during docker build doesn't install dependencies"
type: issue
state: closed
author: VVoruganti
labels:
  - needs-mre
assignees: []
created_at: 2024-10-22T14:39:06Z
updated_at: 2025-02-19T20:24:05Z
url: https://github.com/astral-sh/uv/issues/8460
synced_at: 2026-01-12T15:59:27Z
```

# `uv` sync in workspace during docker build doesn't install dependencies

---

_@VVoruganti_

I have a project that is making use of the `uv` workspaces feature. It has the following structure

```
.
├── pyproject.toml
├── uv.lock
├── bot/
│   └── pyproject.toml
├── api/
│   └── pyproject.toml
└── agent/
    ├── agent/
    ├── pyroject.toml
    └── dist/
```  

This is my root `pyproject.toml`
```toml
[project]
name = "tutor-gpt"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = [
    "honcho-ai>=0.0.15",
    "python-dotenv>=1.0.1",
]

[tool.uv.sources]
agent = { workspace = true }
api = { workspace = true }
bot = { workspace = true }

[tool.uv.workspace]
members = ["agent/", "api/", "bot/"]
exclude = ["www/*"]
```

The `agent` package is installed by the other two apps. 

I need to build a docker image that setups up the `api` package so I modified the cached example in the docs. 

```Dockerfile
FROM python:3.11-slim-bullseye

COPY --from=ghcr.io/astral-sh/uv:0.4.9 /uv /bin/uv

# Set Working directory
WORKDIR /app

RUN addgroup --system app && adduser --system --group app
RUN chown -R app:app /app
USER app

# Enable bytecode compilation
ENV UV_COMPILE_BYTECODE=1

# Copy from the cache instead of linking since it's a mounted volume
ENV UV_LINK_MODE=copy

# Install the project's dependencies using the lockfile and settings
RUN --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    uv sync --frozen --no-install-workspace --no-dev

# Copy only requirements to cache them in docker layer
COPY --chown=app:app pyproject.toml uv.lock  /app/

# Place executables in the environment at the front of the path
ENV PATH="/app/.venv/bin:$PATH"

EXPOSE 8000

COPY --chown=app:app agent/ /app/agent/
COPY --chown=app:app bot/ /app/bot/
COPY --chown=app:app api/ /app/api/

RUN --mount=type=cache,target=/root/.cache/uv \
      uv sync --frozen --package api

CMD ["fastapi", "run", "--host", "0.0.0.0", "api/main.py"]
```

When I run my container I'm getting the following error which indicated to me that none of the packages installed. 
```
docker: Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: exec: "fastapi": executable file not found in $PATH: unknown.
```

I verified this by opening a shell in the container

```
app@612b3a28fa65:/app$ uv pip list
app@612b3a28fa65:/app$ uv sync --package api
Resolved 54 packages in 2ms
Audited in 0.00ms
app@612b3a28fa65:/app$
```

I've noticed that I can force the container to install packages if I remove the `uv.lock` and the `--frozen` tag, but that not desirable for having truly reproducible builds. 

 

---

_Comment by @charliermarsh on 2024-10-22 14:53_

Do you mind providing a reproduction with code, like a Git repo I can clone? I'm happy to help but it's time consuming to scaffold this stuff.

---

_Label `needs-mre` added by @charliermarsh on 2024-10-22 14:56_

---

_Comment by @VVoruganti on 2024-10-23 15:42_

> Do you mind providing a reproduction with code, like a Git repo I can clone? I'm happy to help but it's time consuming to scaffold this stuff.

Yes I've been working out of this repo: https://github.com/plastic-labs/tutor-gpt

---

_Comment by @vilim on 2024-10-25 17:10_

I have a similar issue, and am not able to e.g. run pytest from the second package.
The example is here
https://github.com/vilim/uv-workspace-test
inside the docker container running `uv tree` yields the expected
```
subproject1 v0.1.0
└── pydantic v2.9.2
    ├── annotated-types v0.7.0
    ├── pydantic-core v2.23.4
    │   └── typing-extensions v4.12.2
    └── typing-extensions v4.12.2
subproject2 v0.1.0
└── pytest v8.3.3 (group: dev)
    ├── iniconfig v2.0.0
    ├── packaging v24.1
    └── pluggy v1.5.0
```
however when running pytest I get
```
uv run --package subproject2 pytest

Installed 4 packages in 9ms
Bytecode compiled 98 files in 340ms
============================================================================= test session starts =============================================================================
platform linux -- Python 3.12.7, pytest-8.3.3, pluggy-1.5.0
rootdir: /app
configfile: pyproject.toml
collected 0 items / 1 error                                                                                                                                                   

=================================================================================== ERRORS ====================================================================================
_________________________________________________________________ ERROR collecting subproject2/test_hello.py __________________________________________________________________
ImportError while importing test module '/app/subproject2/test_hello.py'.
Hint: make sure your test modules/packages have valid Python names.
Traceback:
/usr/local/lib/python3.12/importlib/__init__.py:90: in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
subproject2/test_hello.py:1: in <module>
    from hello import main
subproject2/hello.py:1: in <module>
    from subproject1 import module
E   ModuleNotFoundError: No module named 'subproject1'
=========================================================================== short test summary info ===========================================================================
ERROR subproject2/test_hello.py
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! Interrupted: 1 error during collection !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
============================================================================== 1 error in 0.11s ===============================================================================
```
which has two issues, first that the packages get downloaded and installed (again?), and then the dependency is not found.





---

_Comment by @charliermarsh on 2024-10-25 17:11_

I think you need to add a `[build-system]` to `subproject1`. It's not marked as a package right now, so it won't be installed in the environment.

---

_Comment by @vilim on 2024-10-25 17:34_

I added a build system, still the same result

---

_Comment by @charliermarsh on 2024-10-25 17:44_

You have a couple different issues with your project: (1) `subproject2` doesn't declare a dependency on `subproject1`, so it can't import it; and (2) your projects aren't laid out as valid, buildable Python packages.

```diff
diff --git a/.gitignore b/.gitignore
new file mode 100644
index 0000000..b5bbca2
--- /dev/null
+++ b/.gitignore
@@ -0,0 +1,3 @@
+__pycache__
+.pytest_cache
+.venv
diff --git a/subproject1/pyproject.toml b/subproject1/pyproject.toml
index bea73c3..1e483dd 100644
--- a/subproject1/pyproject.toml
+++ b/subproject1/pyproject.toml
@@ -10,4 +10,4 @@ dependencies = [
 
 [build-system]
 requires = ["hatchling"]
-build-backend = "hatchling.build"
\ No newline at end of file
+build-backend = "hatchling.build"
diff --git a/subproject1/src/subproject1/__init__.py b/subproject1/src/subproject1/__init__.py
new file mode 100644
index 0000000..e69de29
diff --git a/subproject1/module.py b/subproject1/src/subproject1/module.py
similarity index 78%
rename from subproject1/module.py
rename to subproject1/src/subproject1/module.py
index 7060ad1..70a8fe4 100644
--- a/subproject1/module.py
+++ b/subproject1/src/subproject1/module.py
@@ -4,4 +4,4 @@ class T(BaseModel):
     a: str
 
 def dependency():
-    return T(a="ABC")
\ No newline at end of file
+    return T(a="ABC")
diff --git a/subproject2/pyproject.toml b/subproject2/pyproject.toml
index 6dbbbd8..9cca09f 100644
--- a/subproject2/pyproject.toml
+++ b/subproject2/pyproject.toml
@@ -4,9 +4,16 @@ version = "0.1.0"
 description = "Add your description here"
 readme = "README.md"
 requires-python = ">=3.11"
-dependencies = []
+dependencies = ["subproject1"]
+
+[tool.uv.sources]
+subproject1 = { workspace = true }
 
 [tool.uv]
 dev-dependencies = [
     "pytest>=8.3.3",
 ]
+
+[build-system]
+requires = ["hatchling"]
+build-backend = "hatchling.build"
diff --git a/subproject2/src/subproject2/__init__.py b/subproject2/src/subproject2/__init__.py
new file mode 100644
index 0000000..e69de29
diff --git a/subproject2/hello.py b/subproject2/src/subproject2/hello.py
similarity index 100%
rename from subproject2/hello.py
rename to subproject2/src/subproject2/hello.py
diff --git a/subproject2/test_hello.py b/subproject2/test_hello.py
deleted file mode 100644
index 5a94c31..0000000
--- a/subproject2/test_hello.py
+++ /dev/null
@@ -1,4 +0,0 @@
-from hello import main
-
-def test_hello():
-    main()
\ No newline at end of file
diff --git a/subproject2/tests/test_hello.py b/subproject2/tests/test_hello.py
new file mode 100644
index 0000000..63081ff
--- /dev/null
+++ b/subproject2/tests/test_hello.py
@@ -0,0 +1,4 @@
+from subproject2.hello import main
+
+def test_hello():
+    main()
diff --git a/uv.lock b/uv.lock
index 021736a..ddb8189 100644
--- a/uv.lock
+++ b/uv.lock
@@ -136,7 +136,7 @@ wheels = [
 [[package]]
 name = "subproject1"
 version = "0.1.0"
-source = { virtual = "subproject1" }
+source = { editable = "subproject1" }
 dependencies = [
     { name = "pydantic" },
 ]
@@ -147,7 +147,10 @@ requires-dist = [{ name = "pydantic", specifier = ">=2.9.2" }]
 [[package]]
 name = "subproject2"
 version = "0.1.0"
-source = { virtual = "subproject2" }
+source = { editable = "subproject2" }
+dependencies = [
+    { name = "subproject1" },
+]
 
 [package.dev-dependencies]
 dev = [
@@ -155,6 +158,7 @@ dev = [
 ]
 
 [package.metadata]
+requires-dist = [{ name = "subproject1", editable = "subproject1" }]
 
 [package.metadata.requires-dev]
 dev = [{ name = "pytest", specifier = ">=8.3.3" }]
```

---

_Comment by @charliermarsh on 2024-10-25 17:46_

Here it is as a PR: https://github.com/vilim/uv-workspace-test/pull/1

---

_Comment by @vilim on 2024-10-25 19:08_

Thank you very much, this works now! (in the original code where I had the issue I had the proper package layout, dependencies and sources configured, but not the build system, adding just the build system makes it work). Maybe it would be a good idea to mention this in the workspace documentation?

---

_Comment by @VVoruganti on 2024-11-05 16:15_

@charliermarsh Can you let me know if any more additional information is needed to reproduce the error I was having. This repo has a the uv workspace defined at the top level: 

https://github.com/plastic-labs/tutor-gpt


---

_Comment by @charliermarsh on 2025-02-14 20:04_

@VVoruganti -- Just checking, is this still relevant? I copied over the Dockerfile above and built + ran the container, and it seemed to find `fastapi` without issue.

---

_Comment by @VVoruganti on 2025-02-19 20:13_

> [@VVoruganti](https://github.com/VVoruganti) -- Just checking, is this still relevant? I copied over the Dockerfile above and built + ran the container, and it seemed to find `fastapi` without issue.

Hey Charlie not relevant anymore we have migrated away from our original architecture. Unsure what the problem was.


---

_Closed by @charliermarsh on 2025-02-19 20:24_

---
