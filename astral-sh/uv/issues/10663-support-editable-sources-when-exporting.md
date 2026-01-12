```yaml
number: 10663
title: Support (editable) sources when exporting requirements.txt when building a Docker image
type: issue
state: open
author: tibbe
labels: []
assignees: []
created_at: 2025-01-16T00:59:02Z
updated_at: 2025-04-09T14:56:01Z
url: https://github.com/astral-sh/uv/issues/10663
synced_at: 2026-01-12T16:00:18Z
```

# Support (editable) sources when exporting requirements.txt when building a Docker image

---

_@tibbe_

We have a monorepo with multiple libraries, some which depend on each other, and multiple executables, which depend on (some of) the libraries. We're trying to build a Docker image for AWS Lambda for each of the executables.

Since not all executables have compatible dependencies we cannot use a workspace as per our testing and as per https://docs.astral.sh/uv/concepts/projects/workspaces/#when-not-to-use-workspaces.

What we do instead is to use (editable) dependencies like so:

```toml
# executable-a/pyproject.toml
[project]
name = "executable-a"
dependencies = [
  "lib-a"
]

[tool.uv.sources]
lib-a = { path = "../lib-a", editable = true }
```

```toml
# lib-a/pyproject.toml
[project]
name = "lib-a"
dependencies = [
  "lib-b"
]

[tool.uv.sources]
lib-b = { path = "../lib-b", editable = true }
```

The folder structure looks something like:

```
.
├── Dockerfile.a
├── Dockerfile.b
├── executable-a
├── executable-b
├── lib-a
└── lib-b
```

The reason we use editable dependencies is that we frequently need to change code in both libraries and executables at the same time and having to `uv sync` between each code change hurts the development flow.

What I _think_ we want to do in our Docker image is something similar to https://github.com/astral-sh/uv/blob/73cade13862c88bf2d5344759e9b1b7035b63743/docs/guides/integration/aws-lambda.md, namely first installing 3rd party dependencies followed by the libraries and the executable.

Issues:
- We need an equivalent of `uv export --no-emit-workspace` for (editable) sources such that we can create a Docker layer of the 3rd party dependencies, including the 3rd party dependencies of the libraries. `--no-sources` doesn't seem to be quite it as we still do want the 3rd party dependencies of the sources.
- We want the result to have no editable dependencies, which seems to happen even if you use `uv export --no-editable`, as long as `editable = true` is defined in `pyproject.toml`.


---

_Comment by @tibbe on 2025-01-16 01:01_

The reason I'm saying that `uv export --no-editable` doesn't work is that if you install the resulting `requirements.txt` using `uv pip install` you do end up with `*.pth` files with relative paths in them for the libraries.

---

_Comment by @tibbe on 2025-01-16 01:03_

Here's the `export` command:

```Dockerfile
WORKDIR /executable-a

RUN --mount=from=uv,source=/uv,target=/bin/uv \
    --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=executable-a/pyproject.toml,target=pyproject.toml \
    --mount=type=bind,source=executable-a/uv.lock,target=uv.lock \
    --mount=type=bind,source=lib-a,target=../lib-a \
    --mount=type=bind,source=lib-b,target=../lib-b \
    uv export --frozen --no-hashes --no-dev -o requirements.txt --no-editable && \
    uv pip install -r requirements.txt && \
    rm requirements.txt
```

---

_Comment by @tibbe on 2025-04-09 14:56_

I still have this problem. What is the best way to structure the installs of dependencies in a monorepo when a single workspace isn't possible (because we have multiple executables and we need editable installs)?

---
