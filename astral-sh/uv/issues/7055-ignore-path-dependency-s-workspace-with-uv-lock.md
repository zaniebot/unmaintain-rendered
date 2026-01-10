---
number: 7055
title: "Ignore path dependency's workspace with `uv lock`"
type: issue
state: open
author: helderco
labels:
  - question
assignees: []
created_at: 2024-09-04T23:40:50Z
updated_at: 2025-06-05T23:22:46Z
url: https://github.com/astral-sh/uv/issues/7055
synced_at: 2026-01-10T01:24:10Z
---

# Ignore path dependency's workspace with `uv lock`

---

_Issue opened by @helderco on 2024-09-04 23:40_

I have a library `dagger-io` that is vendored locally in a `./sdk` directory in a new project `main` (structured as a library):

```toml
# my-module/pyproject.toml

[project]
name = "main"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = ["dagger-io"]

[tool.uv.sources]
dagger-io = { path = "sdk", editable = true }

[build-system]
requires = ["hatchling==1.25.0"]
build-backend = "hatchling.build"
```

The `dagger-io` library has a workspace with a `codegen` member during development, but I don't want that directory to be available when when vendored (copied into `./my-module/sdk`):
```toml
# my-module/sdk/pyproject.toml

[project]
name = "dagger-io"
# ...

[tool.uv]
dev-dependencies = ["codegen"]

[tool.uv.sources]
codegen = { workspace = true }

[tool.uv.workspace]
members = ["codegen"]
```

So, when I create a lock file (`uv lock`) for project `main`, I get the following error:
```
error: Failed to build: `dagger-io @ file:///src/e864914331058348cb7b498167502588bd163773f866206b82eb4c4758730712/sdk`
  Caused by: Failed to parse entry for: `codegen`
  Caused by: Package is not included as workspace package in `tool.uv.workspace`
```

I want to ignore the workspace information in `dagger-io` when installed as a dependency since that workspace is for developing the `dagger-io` library, in a separate repo.

Or is there another solution to this? 

---

_Comment by @charliermarsh on 2024-09-04 23:47_

Do you need to re-lock outside of development? Is it enough just to sync?

---

_Label `question` added by @charliermarsh on 2024-09-05 02:03_

---

_Comment by @helderco on 2024-09-07 00:20_

What I need to lock is `my-module/uv.lock` (new project from template, needs new lock). The vendored library (`dagger-io`) should behave like something you'd install from PyPI, it's just vendored locally because modules need to patch that library slightly (to overwrite a `.py` file).

So currently it's like doing `uv add dagger-io` and expecting uv to pick up on the library's workspace, which is only used to develop the library (i.e., in its own repo, not in `my-module`). 

---

_Comment by @helderco on 2024-09-09 13:44_

To be clear, I think the problem is that I want to vendor without the `codegen` member but `uv lock` fails to parse the `sdk/pyproject.toml` (or `sdk/uv.lock`) without that subdir. It shows in `my-module/uv.lock` but isn't needed there.

**Does `my-module/uv.lock` really need to know the dev dependencies from `my-modules/sdk`?**

This is the difference between `uv lock --no-sources` and having a source with `dagger-io = { path = "sdk" }`:

```diff
[[package]]
name = "dagger-io"
-version = "0.12.7"
-source = { registry = "https://pypi.org/simple" }
+version = "0.0.0"
+source = { directory = "sdk" }
dependencies = [
    { name = "anyio" },
    { name = "beartype" },
    { name = "cattrs" },
    { name = "gql", extra = ["httpx"] },
    { name = "opentelemetry-exporter-otlp-proto-grpc" },
    { name = "opentelemetry-sdk" },
    { name = "platformdirs" },
    { name = "rich" },
    { name = "typing-extensions" },
]
-sdist = { url = "https://files.pythonhosted.org/packages/10/5d/99cf39497c00ea3869e92246e364e0480979788d27388ed33273c03865cb/dagger_io-0.12.7.tar.gz", hash = "sha256:49a50644a269a4fd785481a3b9a6a8a08c98caf2b4c835ea65e8c0fdbb641bf6", size = 81419 }
-wheels = [
-    { url = "https://files.pythonhosted.org/packages/49/8d/878003f142338ca9e535c38044ee63fdae446b161c83366b734831551323/dagger_io-0.12.7-py3-none-any.whl", hash = "sha256:69e7986b04e069b9a19805010d17ead8c0153eaf0be9d3dacf87585eb7d3ba7f", size = 76526 },
+
+[package.metadata]
+requires-dist = [
+    { name = "anyio", specifier = ">=3.6.2" },
+    { name = "beartype", specifier = ">=0.18.2" },
+    { name = "cattrs", specifier = ">=22.2.0" },
+    { name = "gql", extras = ["httpx"], specifier = ">=3.5.0" },
+    { name = "opentelemetry-exporter-otlp-proto-grpc", specifier = ">=1.23.0" },
+    { name = "opentelemetry-sdk", specifier = ">=1.23.0" },
+    { name = "platformdirs", specifier = ">=2.6.2" },
+    { name = "rich", specifier = ">=10.11.0" },
+    { name = "typing-extensions", specifier = ">=4.8.0" },
+]
+
+[package.metadata.requires-dev]
+dev = [
+    { name = "aiohttp", specifier = ">=3.9.3" },
+    { name = "codegen", editable = "sdk/codegen" },
+    { name = "mypy", specifier = ">=1.8.0" },
+    { name = "pytest", specifier = ">=8.0.2" },
+    { name = "pytest-httpx", specifier = ">=0.30.0" },
+    { name = "pytest-mock", specifier = ">=3.12.0" },
+    { name = "pytest-subprocess", specifier = ">=1.5.0" },
+    { name = "ruff", specifier = ">=0.3.4" },
+    { name = "sphinx", specifier = ">=7.2.6" },
+    { name = "sphinx-rtd-theme", specifier = ">=2.0.0" },
]
```

> [!note]
> The published version (0.12.7) has uv dev dependencies (but `uv.lock` isn’t published):
>
> https://github.com/dagger/dagger/blob/9823a005d5c1c131345c3278bd6ef197c65d1c8b/sdk/python/pyproject.toml#L53-L66

So I suppose I don’t get the dev dependencies if the source is already built (i.e., uv doesn’t have to build before install). But even with `uv build` I still don’t get them:

```
rm -rf sdk/codegen
uv build sdk
uv add --no-binary-package dagger-io ./sdk/dist/dagger_io-0.0.0.tar.gz
```

I used `--no-binary-package dagger-io` because I wanted to feed something that has the `pyproject.toml` and `uv.lock` from the `dagger-io` library to `uv add`, to see if the dev dependencies would get in the `my-module/uv.lock`, but they don’t:

```diff
[[package]]
name = "dagger-io"
-version = "0.12.7"
-source = { registry = "https://pypi.org/simple" }
+version = "0.0.0"
+source = { path = "sdk/dist/dagger_io-0.0.0.tar.gz" }
dependencies = [
    { name = "anyio" },
    { name = "beartype" },
    { name = "cattrs" },
    { name = "gql", extra = ["httpx"] },
    { name = "opentelemetry-exporter-otlp-proto-grpc" },
    { name = "opentelemetry-sdk" },
    { name = "platformdirs" },
    { name = "rich" },
    { name = "typing-extensions" },
]
-sdist = { url = "https://files.pythonhosted.org/packages/10/5d/99cf39497c00ea3869e92246e364e0480979788d27388ed33273c03865cb/dagger_io-0.12.7.tar.gz", hash = "sha256:49a50644a269a4fd785481a3b9a6a8a08c98caf2b4c835ea65e8c0fdbb641bf6", size = 81419 }
-wheels = [
-    { url = "https://files.pythonhosted.org/packages/49/8d/878003f142338ca9e535c38044ee63fdae446b161c83366b734831551323/dagger_io-0.12.7-py3-none-any.whl", hash = "sha256:69e7986b04e069b9a19805010d17ead8c0153eaf0be9d3dacf87585eb7d3ba7f", size = 76526 },
-]
+
+[package.metadata]
+requires-dist = [
+    { name = "anyio", specifier = ">=3.6.2" },
+    { name = "beartype", specifier = ">=0.18.2" },
+    { name = "cattrs", specifier = ">=22.2.0" },
+    { name = "gql", extras = ["httpx"], specifier = ">=3.5.0" },
+    { name = "opentelemetry-exporter-otlp-proto-grpc", specifier = ">=1.23.0" },
+    { name = "opentelemetry-sdk", specifier = ">=1.23.0" },
+    { name = "platformdirs", specifier = ">=2.6.2" },
+    { name = "rich", specifier = ">=10.11.0" },
+    { name = "typing-extensions", specifier = ">=4.8.0" },
+]
```

**Only the production dependencies are included, which is what I want.**

If I use the local directory as the source, it assumes I want to develop (i.e., tries to load the workspace), even without `--editable`: 

```
✗  uv add ./sdk
error: Failed to build: `dagger-io @ file:///Users/helder/Projects/dagger/sdk/python/dev/sdk`
  Caused by: Failed to parse entry for: `codegen`
  Caused by: Package is not included as workspace package in `tool.uv.workspace`
```

What I expected was the same as `uv build sdk` does. It doesn’t fail due to the missing workspace package and only adds `dagger-io`’s production dependencies to the lock.

> [!note]
> Even though I use `uv add` in these examples, what I’m trying to do is just `uv lock` in `my-module` since the `pyproject.toml` already includes the right config, added from a template.

The reason I need to vendor the library is because I need to change a source code file due to a code generation step (in particular, the `sdk/src/dagger/client/gen.py` file). And the reason I need `--editable` is to have that change locally as well as in the OCI container, without having to install it again on changes (and without having to handle different `site-package` locations).

In `my-module` I don’t want `my-module/sdk` dev dependencies nor share a `.venv`. If I wanted to develop them together, I’d define a workspace in `my-module/pyproject.toml`, which **is what seems to be documented in [When (not) to use workspaces](https://docs.astral.sh/uv/concepts/workspaces/#when-not-to-use-workspaces)**:

> Workspaces are _not_ suited for cases in which members have conflicting requirements, or desire a separate virtual environment for each member. In this case, path dependencies are often preferable.
> […]
> This approach conveys many of the same benefits, but allows for more fine-grained control over dependency resolution and virtual environment management

But still, is there a way to distinguish when I want a path dependency’s dev dependencies vs not?

---

_Renamed from "Ignore workspace in local dependency with `uv lock`" to "Ignore workspace in path dependency, with `uv lock`" by @helderco on 2024-09-09 13:44_

---

_Renamed from "Ignore workspace in path dependency, with `uv lock`" to "Ignore path dependency's workspace with `uv lock`" by @helderco on 2024-09-09 13:45_

---

_Comment by @zanieb on 2024-10-21 21:43_

Is this fixed now that we don't sync child project dependencies?

---

_Comment by @helderco on 2024-11-02 19:30_

No:
```console
❯ uv --verbose lock
DEBUG uv 0.4.29 (85f9a0d0e 2024-10-30)
DEBUG Found workspace root `/Users/helder/Projects/dagger/sdk/python`, but project is not included
DEBUG Found workspace root: `/Users/helder/Projects/dagger/sdk/python/dev`
DEBUG Adding current workspace member: `/Users/helder/Projects/dagger/sdk/python/dev`
DEBUG The virtual environment's Python version satisfies `Python >=3.12`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: main @ file:///Users/helder/Projects/dagger/sdk/python/dev
DEBUG Found workspace root `/Users/helder/Projects/dagger/sdk/python`, but project is not included
DEBUG No workspace root found, using project root
DEBUG No static `pyproject.toml` available for: dagger-io @ file:///Users/helder/Projects/dagger/sdk/python/dev/sdk (PyprojectToml(DynamicField("version")))
DEBUG Acquired lock for `/Users/helder/.cache/uv/sdists-v5/editable/81158b3d3e6e8b9a`
DEBUG Using cached metadata for: dagger-io @ file:///Users/helder/Projects/dagger/sdk/python/dev/sdk
DEBUG Found workspace root: `/Users/helder/Projects/dagger/sdk/python/dev/sdk`
DEBUG Adding current workspace member: `/Users/helder/Projects/dagger/sdk/python/dev/sdk`
DEBUG Released lock at `/Users/helder/.cache/uv/sdists-v5/editable/81158b3d3e6e8b9a/.lock`
error: Failed to generate package metadata for `dagger-io==0.0.0 @ editable+sdk`
  Caused by: Failed to parse entry in group `dev`: `codegen`
  Caused by: Package is not included as workspace package in `tool.uv.workspace`
```

For this use case:
- `dagger-io` has a `codegen` dev dependency in a child project/member of that workspace. That doesn't matter when published to `pypi`
- `./` depends on `./sdk`, which is a vendored `dagger-io`, without the `codegen` subdir
- `./` shouldn't require that `./sdk/codegen` exists and `./sdk` shouldn't be a part of its workspace either


---

_Comment by @mluis on 2025-05-21 12:34_

@helderco , is there any workaround for this?

Thanks in advance.

---

_Comment by @helderco on 2025-06-05 23:22_

Hey @mluis! 😃 Not that I know of. I ended up including that `codegen` sub-package, but would rather not still, if I could.

---
