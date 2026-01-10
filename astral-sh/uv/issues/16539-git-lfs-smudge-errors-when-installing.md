---
number: 16539
title: Git LFS Smudge Errors When Installing Dependencies from Git Sources with LFS Files
type: issue
state: closed
author: ekzhu
labels:
  - bug
assignees: []
created_at: 2025-10-31T17:25:13Z
updated_at: 2025-12-16T13:34:38Z
url: https://github.com/astral-sh/uv/issues/16539
synced_at: 2026-01-10T01:26:07Z
---

# Git LFS Smudge Errors When Installing Dependencies from Git Sources with LFS Files

---

_Issue opened by @ekzhu on 2025-10-31 17:25_

## Summary

When installing a Python package from a Git repository that contains Git LFS tracked files using `uv sync` or `uv add`, the installation fails with Git LFS smudge errors, even when the LFS objects exist on the remote. Regular `git clone` works fine for the same repository.

## Related Issue

This issue was discovered while investigating: https://github.com/microsoft/agent-framework/issues/1832

## Steps to Reproduce

1. Create a minimal `pyproject.toml` with a git dependency from a repository containing LFS files:

```toml
[project]
name = "test-project"
version = "0.1.0"
dependencies = [
    "agent-framework-mem0 @ git+https://github.com/microsoft/agent-framework.git#subdirectory=python/packages/mem0"
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

2. Run `uv sync`

## Expected Behavior

The repository should be cloned successfully with all Git LFS files downloaded, similar to how `git clone https://github.com/microsoft/agent-framework.git` works.

## Actual Behavior

The installation fails with a Git LFS smudge error:

```
Error downloading object: python/packages/lab/lightning/assets/train_math_agent.png (ab35a2b):
Smudge error: Error downloading python/packages/lab/lightning/assets/train_math_agent.png
(ab35a2bd18794a32b76437671ae7e5749992c8aa781030b51eca2e56acfb362d):
error transferring "ab35a2bd18794a32b76437671ae7e5749992c8aa781030b51eca2e56acfb362d":
[0] remote missing object ab35a2bd18794a32b76437671ae7e5749992c8aa781030b51eca2e56acfb362d

error: external filter 'git-lfs filter-process' failed
fatal: python/packages/lab/lightning/assets/train_math_agent.png: smudge filter lfs failed
```

This error persists even after:
- Clearing uv cache (`uv cache clean`)
- Verifying the LFS object exists on GitHub (regular `git clone` works fine)
- Confirming the SHA256 hash matches in successful clones

### Platform

Ubuntu 22.04.5 LTS (WSL2)

### Version

0.8.19

### Python version

3.13.0

---

_Label `bug` added by @ekzhu on 2025-10-31 17:25_

---

_Referenced in [microsoft/agent-framework#1832](../../microsoft/agent-framework/issues/1832.md) on 2025-10-31 17:27_

---

_Comment by @samypr100 on 2025-11-01 03:17_

https://github.com/astral-sh/uv/pull/16143 should resolve this (confirmed) as it behaves well regardless of filters.

I believe this is a case that even with setting `UV_GIT_LFS=1` it would not be resolved (with current uv).

In the mean time, the issue could also likely be the Git LFS filters configuration.
Make sure you run `git lfs install` or that your `~/.gitconfig` at least has filters properly configured.
You may also want to double check your system Git LFS config (in case it has conflicts) under `/etc/gitconfig`.

```
[filter "lfs"]
        clean = git-lfs clean -- %f
        smudge = git-lfs smudge -- %f
        process = git-lfs filter-process
...
```

---

_Referenced in [astral-sh/uv#16143](../../astral-sh/uv/pulls/16143.md) on 2025-11-04 03:05_

---

_Closed by @konstin on 2025-12-16 13:34_

---
