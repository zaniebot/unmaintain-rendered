```yaml
number: 10523
title: Better support for reproducible Docker images and other artifacts?
type: issue
state: open
author: maxfriedrich
labels:
  - enhancement
  - great writeup
assignees: []
created_at: 2025-01-11T21:07:28Z
updated_at: 2025-01-15T09:22:21Z
url: https://github.com/astral-sh/uv/issues/10523
synced_at: 2026-01-12T16:00:15Z
```

# Better support for reproducible Docker images and other artifacts?

---

_@maxfriedrich_

Hi, I did some research regarding reproducibility of Python artifacts recently to speed up deployments at my work by skipping unnecessary redeploys. I saw the [Lambda integration docs](https://docs.astral.sh/uv/guides/integration/aws-lambda/) were updated to include `UV_NO_INSTALLER_METADATA=1` so uv doesn't write metadata like timestamps, which is a good step in this direction.

However the output still changes with every build that doesn't use the (same) cache, like on CI or a different developer's machine.

The things that change are:
* all bytecode files, a timestamp is written into the header of each one to decide when to invalidate it
* the file timestamps, with `UV_LINK_MODE=copy` they are set to the time they are copied

It is possible to work around this to create a build that "reproducible" in the sense that

```bash
docker build --no-cache -t a1 . && docker build --no-cache -t a2 . && \
docker inspect a1 > a1.txt && docker inspect a2 > a2.txt && \
diff a1.txt a2.txt
```

only includes the image metadata but the layers (e.g. `jq ".[0].RootFS" a1.txt`) are the same. Then, as far as I understand, pushing to a container registry would output "layer already exists" and skip the upload and depending on the deployment, allow skipping the deployment altogether (like with AWS CDK).

This is an updated Dockerfile for the first example in the docs that achieves this, but in a very awkward way: https://gist.github.com/maxfriedrich/19b933dc5b35a08d09dc8a27c905b829

Diff to the example from the docs:

```diff
 FROM ghcr.io/astral-sh/uv:0.5.18 AS uv
 
 # First, bundle the dependencies into the task root.
 FROM public.ecr.aws/lambda/python:3.13 AS builder
 
 # Enable bytecode compilation, to improve cold-start performance.
 ENV UV_COMPILE_BYTECODE=1
 
 # Disable installer metadata, to create a deterministic layer.
 ENV UV_NO_INSTALLER_METADATA=1
 
+# Setting source date epoch to any value enables hash-based Python bytecode invalidation
+# instead of timestamp-based for reproducibility.
+# Here, we also use the value to set the timestamps of dependencies and application code.
+ENV SOURCE_DATE_EPOCH=0
+
 # Enable copy mode to support bind mount caching.
 ENV UV_LINK_MODE=copy
 
 # Bundle the dependencies into the Lambda task root via `uv pip install --target`.
 #
 # Omit any local packages (`--no-emit-workspace`) and development dependencies (`--no-dev`).
 # This ensures that the Docker layer cache is only invalidated when the `pyproject.toml` or `uv.lock`
 # files change, but remains robust to changes in the application code.
 RUN --mount=from=uv,source=/uv,target=/bin/uv \
     --mount=type=cache,target=/root/.cache/uv \
     --mount=type=bind,source=uv.lock,target=uv.lock \
     --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
     uv export --frozen --no-emit-workspace --no-dev --no-editable -o requirements.txt && \
-    uv pip install -r requirements.txt --target "${LAMBDA_TASK_ROOT}"
+    uv pip install -r requirements.txt --target "${LAMBDA_TASK_ROOT}" && \
+    # `find` is not available in the base image, so we use Python to walk the path and set timestamps.
+    python -c "import os; from pathlib import Path; \
+    base = Path(os.environ['LAMBDA_TASK_ROOT']); \
+    ts = int(os.environ['SOURCE_DATE_EPOCH']); \
+    os.utime(base, (ts, ts)); \
+    [os.utime(p, (ts, ts)) for p in base.rglob('*') if p.name not in ('pyproject.toml', 'uv.lock')]"
 
-FROM public.ecr.aws/lambda/python:3.13
+# Copy the application code into the builder image and set the timestamps.
+# This layer is invalidated when the application code changes.
+COPY app "${LAMBDA_TASK_ROOT}"/app
+RUN python -c "import os; from pathlib import Path; \
+    base = Path(os.environ['LAMBDA_TASK_ROOT']); \
+    app = base / 'app'; \
+    ts = int(os.environ['SOURCE_DATE_EPOCH']); \
+    os.utime(base, (ts, ts)); \
+    os.utime(app, (ts, ts)); \
+    [os.utime(p, (ts, ts)) for p in app.rglob('*')]"
 
-# Copy the runtime dependencies from the builder stage.
-COPY --from=builder ${LAMBDA_TASK_ROOT} ${LAMBDA_TASK_ROOT}
 
-# Copy the application code.
-COPY ./app ${LAMBDA_TASK_ROOT}/app
+FROM public.ecr.aws/lambda/python:3.13
+
+# Copy the runtime dependencies and application code into the final image.
+COPY --from=builder "${LAMBDA_TASK_ROOT}" "${LAMBDA_TASK_ROOT}"
 
 # Set the AWS Lambda handler.
 CMD ["app.main.handler"]
```

This is obviously not ideal, I think explicit support for something like this in uv would help out a lot.

Concretely, I would suggest: if `UV_LINK_MODE=copy` and `SOURCE_DATE_EPOCH` is set to any value, `uv pip install [--target] ...` could set the timestamps of all written files to the `SOURCE_DATE_EPOCH`. It's a [standardized environment variable](https://reproducible-builds.org/docs/source-date-epoch/) for purposes like this and the Python bytecode compiler already reacts to it, but it would be kind of a breaking change.

The same could also be applied to `uv sync` commands.

For the second step of copying the application code, uv is not playing any part, so there is nothing to improve there. In my own deployments, I'm using a workspace of library packages and running `uv sync` once for dependencies and once for application code to build Lambda artifacts, so this behavior change would mean I wouldn't have to update any file timestamps.

I just used an example from the Lambda docs, I think this also applies to Docker builds in general.

What do you think? Is this something that you would like to support? Thank you!

---

_Label `enhancement` added by @zanieb on 2025-01-11 21:21_

---

_Label `great writeup` added by @zanieb on 2025-01-11 21:21_

---

_Comment by @jackvreeken on 2025-01-14 18:45_

That would be very useful indeed, and an issue I'm running into now as well. Unfortunately I can't really help with the solution, but I do have a related question for you if you don't mind me asking. I saw your great write-up of bundling Lambdas in CDK using uv in [your blog post](https://maxfriedrich.de/2025/01/02/uv-lambda-cdk/) , and was wondering if you know how to apply a similar strategy for Lambda Docker images? 

The [uv docs](https://docs.astral.sh/uv/guides/integration/aws-lambda/) do not cover the case that is seemingly more common for me (and you as well I think based on your blog post). In other words, the Lambdas that I want to package are workspace **members**. If we're not the odd ones out, it would be great to include this in the docs somewhere in the "Workspace support" section.

Where would you put the Dockerfiles in this hypothetical case? Would you `docker buildx build -f packages/demo-lambda1/Dockerfile .` just to get the build context to include the `uv.lock` and workspace `pyproject.toml` files?

```
.
├── cdk
│   ├── app.py
│   ├── cdk.json
│   ├── pyproject.toml
│   └── python_lambda_function.py
├── packages
│   ├── demo-common
│   │   ├── pyproject.toml
│   │   └── src
│   │       └── demo_common
│   │           └── __init__.py
│   ├── demo-lambda1
│   │   ├── Dockerfile
│   │   ├── pyproject.toml
│   │   └── src
│   │       └── demo_lambda1
│   │           ├── __init__.py
│   │           └── lambda_function.py
│   └── demo-lambda2 
│       ├── Dockerfile
│       └── ...
├── pyproject.toml
└── uv.lock

```


EDIT:
Maybe I did find something, at least it _seems_ to work:

Instead of the Python code you added, I can do `docker buildx build [.....] --build-arg SOURCE_DATE_EPOCH=0 --output type=docker,buildinfo=false,rewrite-timestamp=true` . Most notably, the combination of `rewrite-timestamp=true` + `build-arg SOURCE_DATE_EPOCH=0`. See https://github.com/moby/buildkit/pull/4057 

---

_Comment by @zanieb on 2025-01-15 01:00_

Regarding the bytecode files, it looks like we respect [`PYC_INVALIDATION_MODE`](https://docs.python.org/3/library/py_compile.html#py_compile.PycInvalidationMode)

https://github.com/astral-sh/uv/blob/a6602ad416a922060efb3c5f19daf4977c50ace3/crates/uv-installer/src/pip_compileall.py#L25

---

_Comment by @maxfriedrich on 2025-01-15 09:15_

@jackvreeken Nice, better than the Python command for sure! GitHub Actions doesn't have BuildKit enabled by default so I think it would still be good to have support in uv. Then we also wouldn't need to rewrite all timestamps, just the ones that matter (because they change)

If you're on the Astral Discord, we can talk about the file layout there… I was using only one Dockerfile for all packages and I put it in the root.

---

_Comment by @maxfriedrich on 2025-01-15 09:22_

@zanieb I think this is correct / useful behavior, the compileall module picks the default/fallback based on SOURCE_DATE_EPOCH being set: https://docs.python.org/3/library/compileall.html#cmdoption-compileall-invalidation-mode and https://github.com/python/cpython/blob/6e4f64109b0eb6c9f1b50eb7dc5f647a1d901ff4/Lib/py_compile.py#L72

The file creation/modification timestamps still need my `python -c` or @jackvreeken's `docker ... rewrite-timestamps=true` workaround

---
