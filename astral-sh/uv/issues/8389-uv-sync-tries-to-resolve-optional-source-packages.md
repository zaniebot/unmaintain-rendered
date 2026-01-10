---
number: 8389
title: "`uv sync` tries to resolve optional source packages even though environment markers exclude the package from the environment"
type: issue
state: closed
author: benniekiss
labels:
  - question
assignees: []
created_at: 2024-10-20T18:23:51Z
updated_at: 2024-10-23T02:19:59Z
url: https://github.com/astral-sh/uv/issues/8389
synced_at: 2026-01-10T01:24:27Z
---

# `uv sync` tries to resolve optional source packages even though environment markers exclude the package from the environment

---

_Issue opened by @benniekiss on 2024-10-20 18:23_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
When running `uv sync` on a project that contains platform-specific optional dependencies built from source, the sync fails when those dependencies cannot be built for the platform during package resolution. `uv sync` does not respect the environment marker during package resolution, and since all optional dependencies are included in the resolution, the environment cannot be synced.

Is it possible to exclude certain packages during resolution?

In the below example, flash-attention requires CUDA, which is not available for macOS, but when trying to sync the project, resolution fails because it attempts to build flash-attention first.

```shell
mkdir -p test_uv_sync
cd test_uv_sync

## make pyproject.toml
cat <<EOF > pyproject.toml
[project]
name = "test_uv_sync"
version = "0.0.1"

dependencies = [
    "torch>=2.4.0,<2.5.0",
    "torchaudio>=2.4.0,<2.5.0",
    "torchvision>=0.19.0,<0.20.0",
]
readme = "README.md"
requires-python = ">= 3.12"

[project.optional-dependencies]
flash = [
    "flash-attn @ git+https://github.com/Dao-AILab/flash-attention ; platform_system == 'Darwin'"
]
EOF

## try to sync project
uv sync --python 3.12
```


---

_Comment by @benniekiss on 2024-10-20 21:35_

Here's a better repro:

```shell
mkdir -p test_uv_sync
cd test_uv_sync

## make pyproject.toml
cat <<EOF > pyproject.toml
[project]
name = "test_uv_sync"
version = "0.0.1"

dependencies = [
    "rich",
]
requires-python = ">= 3.8"

[project.optional-dependencies]
black = [
    "black @ git+https://github.com/psf/black  ; python_version > '3.8'",
]
EOF

## try to sync project
uv sync --python 3.8
```

---

_Comment by @charliermarsh on 2024-10-21 00:20_

It's somewhat fundamental to the design of the lockfile that we resolve for all platforms, not just the current platform -- I don't think there's a way to omit it like you're looking for here. I'd probably suggest locking on Linux then?

---

_Comment by @zanieb on 2024-10-21 00:27_

For details, see

- https://docs.astral.sh/uv/concepts/resolution/#universal-resolution
- https://docs.astral.sh/uv/concepts/projects/#limited-resolution-environments

---

_Label `question` added by @zanieb on 2024-10-21 00:27_

---

_Comment by @konstin on 2024-10-21 09:18_

To add more specific context: uv tries to avoid building during resolution, especially for non-native packages. For git repositories, uv reads static `pyproject.toml` files, so if flash attention would provide name, version and dependencies statically in `pyproject.toml`, we could skip building it.

For cases where the other options don't work, we have an escape hatch through https://docs.astral.sh/uv/concepts/resolution/#dependency-metadata where you can set the manually: It's dangerous because if you specify it wrongly, you get a strangely broken venv, but it may allow you to work around the build problem.

---

_Comment by @benniekiss on 2024-10-21 13:40_

Thank you all, this is really helpful. It looks like setting dependency metadata is the solution I'm looking for, and the docs even use flash-attn as an example.

I completely missed that section of the docs when reading, and I think it would be helpful to include a link or note to the dependency metadata section under the https://docs.astral.sh/uv/concepts/projects/#limited-resolution-environments section.

---

_Comment by @benniekiss on 2024-10-21 19:24_

I'm struggling to get the dependency metadata override working when specifying a git source, and it still seems to be trying to build the package. I'm not sure what I'm doing wrong here, if you all are able to give me any insight.

Here are three examples:

In this instance, `uv` is able to lock and sync the environment, though I was not expecting this:
```shell
mkdir -p test_uv_sync
cd test_uv_sync

## make pyproject.toml
cat <<EOF > pyproject.toml
[project]
name = "test_uv_sync"
version = "0.0.1"

dependencies = [
    "rich",
]
requires-python = ">= 3.8"

[project.optional-dependencies]
black = [
    "black ; python_version > '3.8'",
]
EOF

## try to sync project
uv sync --python 3.8
```

In this instance, `uv` is not able to lock and sync the environment, as expected:
```shell
mkdir -p test_uv_sync
cd test_uv_sync

## make pyproject.toml
cat <<EOF > pyproject.toml
[project]
name = "test_uv_sync"
version = "0.0.1"

dependencies = [
    "rich",
]
requires-python = ">= 3.8"

[project.optional-dependencies]
black = [
    "black ; python_version > '3.8'",
]

[tool.uv.sources]
black = { git = "https://github.com/psf/black" }
EOF

## try to sync project
uv sync --python 3.8
```

And finally, even though `[[tool.uv.dependency-metadata]]` is declared, `uv` still fails to resolve the environment:
```shell
mkdir -p test_uv_sync
cd test_uv_sync

## make pyproject.toml
cat <<EOF > pyproject.toml
[project]
name = "test_uv_sync"
version = "0.0.1"

dependencies = [
    "rich",
]
requires-python = ">= 3.8"

[project.optional-dependencies]
black = [
    "black ; python_version > '3.8'",
]

[tool.uv.sources]
black = { git = "https://github.com/psf/black" }

[[tool.uv.dependency-metadata]]
name = "black"
requires-python = ">=3.8"
requires-dist = [
  "click>=8.0.0",
  "mypy_extensions>=0.4.3",
  "packaging>=22.0",
  "pathspec>=0.9.0",
  "platformdirs>=2",
  "tomli>=1.1.0; python_version < '3.11'",
  "typing_extensions>=4.0.1; python_version < '3.11'",
]
EOF

## try to sync project
uv sync --python 3.8
```



---

_Comment by @charliermarsh on 2024-10-21 19:46_

Dependency overrides are not supported for non-registry dependencies right now.

---

_Comment by @charliermarsh on 2024-10-21 19:47_

There's an open PR to enable them: https://github.com/astral-sh/uv/pull/7846

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-23 02:18_

---

_Comment by @charliermarsh on 2024-10-23 02:19_

Closed by https://github.com/astral-sh/uv/pull/7846. I also added `flash-attn` (from Git) to that PR's summary as an example.

---

_Closed by @charliermarsh on 2024-10-23 02:19_

---

_Referenced in [astral-sh/uv#15715](../../astral-sh/uv/issues/15715.md) on 2025-09-08 01:40_

---
