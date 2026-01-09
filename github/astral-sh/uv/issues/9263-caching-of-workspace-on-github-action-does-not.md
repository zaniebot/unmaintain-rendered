---
number: 9263
title: Caching of workspace on Github Action does not work well.
type: issue
state: open
author: higumachan
labels: []
assignees: []
created_at: 2024-11-20T03:52:07Z
updated_at: 2025-12-10T22:35:22Z
url: https://github.com/astral-sh/uv/issues/9263
synced_at: 2026-01-07T13:12:18-06:00
---

# Caching of workspace on Github Action does not work well.

---

_Issue opened by @higumachan on 2024-11-20 03:52_

When creating a workspace with multiple sub project, the cache for subdirectories does not work as expected.

## uv version

```
0.5.3
```

## Prerequisites

For example, when running the following in a GitHub Action:

```
      - name: "Install the project"
        run: uv sync --no-editable --all-extras --dev --frozen -vvv  --all-packages
        env:
          UV_CONCURRENT_BUILDS: 1
          UV_CONCURRENT_DOWNLOADS: 1
          UV_CONCURRENT_INSTALLS: 1
```

This issue occurs in projects using uv with multiple `pyproject.toml` files, such as the following structure:

```
.
├── pyproject.toml
└── src/
    └── packages/
        ├── package1/
        │   ├── src
        │   └── pyproject.toml
        └── package2/
            ├── src
            └── pyproject.toml
```

Here is an example `pyproject.toml`:

```toml
[project]
name = "some_project"
version = "0.1.0"
requires-python = ">=3.12"
classifiers = [
    "Programming Language :: Rust",
    "Programming Language :: Python :: Implementation :: CPython",
    "Programming Language :: Python :: Implementation :: PyPy",
]
dependencies = [
    "package1",
    "package2",
]
```

## Observed Behavior

Even when configuring the uv cache directory on GitHub Actions, the cache is not applied correctly within uv.

## Cause

The issue seems to stem from a combination of two factors:
1. **Timestamp updates during `git clone`**: Files managed by Git have their timestamps updated to the clone time. As a result, the source files are always considered "newer" than the cache files, causing uv to bypass the cache.
2. **Reliance on `ctime` in uv**: Since uv uses `ctime` to determine cache freshness, workarounds like using the `touch` command cannot be applied.

## Ideas

### Use hash-based cache management in CI mode

Similar to the `--ci` option for `cache prune`, a `--ci` option for `sync` and `run` commands could enable hash-based cache management instead of timestamp-based.

### Use `mtime` instead of `ctime`

Using `mtime` would allow workarounds such as `touch` to function. However, as noted in #1060, this approach may have its own drawbacks and is not ideal.

---

If there are other potential solutions, please share! Especially if there's a way to resolve this issue with the current version of uv.

---

_Comment by @higumachan on 2024-11-20 04:09_

### Use `mtime` with `ctime`

Adopt `mtime` and `ctime` which is ahead in time.


---

_Comment by @ottowayi on 2025-12-10 22:35_

I'm having issues with this as well in Azure DevOps pipelines.  I have a workspace with ~175 members in a pipeline with a couple dozen jobs.  Using the pipeline caching it was taking a couple seconds to recreate the venv in each job, but that was on self-hosted agents where I believe the repo persists between jobs.  But on cloud agents, each job is a fresh clone of the repo and the timestamps don't match the ones in the cache from the previous job.  With `setuptools` this added ~2mins per job, I was able to get it down to ~45s with switch to `hatchling`, but that's still more that doubling my build times.  I had tried between the git checkout and calling `uv sync`, having it override the `cache-keys` in every `pyproject.toml` to just be the git commit one, but that didn't appear to have any effect.  




---
