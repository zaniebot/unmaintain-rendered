---
number: 15298
title: UV does not fall back to other indexes when package lacks platform-compatible wheels
type: issue
state: open
author: tejal567
labels:
  - enhancement
assignees: []
created_at: 2025-08-15T06:45:49Z
updated_at: 2025-08-28T07:22:11Z
url: https://github.com/astral-sh/uv/issues/15298
synced_at: 2026-01-10T01:25:55Z
---

# UV does not fall back to other indexes when package lacks platform-compatible wheels

---

_Issue opened by @tejal567 on 2025-08-15 06:45_

### Summary

## Problem Description

When using multiple package indexes, UV fails to fall back to alternative indexes if a package exists in the first index but doesn't have wheels compatible with the current platform. This differs from Poetry's behavior and can cause installation failures for packages that should be available.

## Expected Behavior

When a package exists in multiple indexes:
1. UV finds the package in the first index
2. If that version lacks wheels for the current platform
3. UV should check other configured indexes for the same version with compatible wheels
4. UV should install from whichever index has platform-compatible wheels

## Actual Behavior

Currently UV:
1. Finds the package in the first index
2. Attempts to install from that index only
3. Fails with platform compatibility error
4. Does not check other indexes for compatible wheels

## Minimal Reproduction Example

**uv.toml:**
```toml
index-strategy = "unsafe-best-match"

[[index]]
name = "private-repo"
url = "https://private-repo.company.com/simple"

[[index]] 
name = "pypi"
url = "https://pypi.org/simple/"
default = true
```

**pyproject.toml:**
```toml
[project]
name = "test-project"
dependencies = ["numpy==2.2.6"]
```

**Scenario:**
- Private repository has `numpy==2.2.6` with only Linux x86_64 wheels
- PyPI has `numpy==2.2.6` with macOS ARM64 wheels  
- Running `uv sync` on macOS ARM64

**Result with both indexes:**
```
error: Distribution `numpy==2.2.6 @ registry+https://private-repo.company.com/simple` 
can't be installed because it doesn't have a source distribution or wheel for the current platform
```

**Result with only PyPI (private repo commented out):**
```
✅ Successfully installed numpy==2.2.6 from PyPI (ARM64 wheels available)
```

This proves that PyPI has the compatible wheels, but UV doesn't fall back when the private repository lacks them.

## Comparison with Poetry

Poetry handles this scenario correctly:
- Finds `numpy==2.2.6` in private repository
- Detects lack of ARM64 macOS wheels
- Automatically falls back to PyPI for the same version
- Successfully installs compatible wheels

## Impact

This limits UV adoption in enterprise environments where:
- Private repositories contain internal packages + some public packages
- Public packages in private repos may have limited platform support
- Teams expect automatic fallback to public PyPI for missing platforms
- Mixed development environments (Linux CI + macOS/Windows developers)

This behavior makes UV less flexible than Poetry for enterprises with mixed repository setups and multi-platform development teams.

### Platform

Darwin 24.6.0 arm64 (MacOS)

### Version

uv 0.8.11 (Homebrew 2025-08-14)

### Python version

Python 3.10.17

---

_Label `bug` added by @tejal567 on 2025-08-15 06:45_

---

_Comment by @charliermarsh on 2025-08-15 10:44_

It probably makes sense to support this (when `unsafe-best-match` is set). It'd be nice to see more demand though -- I don't think it's ever come up before.

---

_Label `bug` removed by @charliermarsh on 2025-08-15 12:46_

---

_Label `enhancement` added by @charliermarsh on 2025-08-15 12:46_

---

_Comment by @tejal567 on 2025-08-15 14:05_

@charliermarsh ah, we would have adopted uv in our company if not for this issue. 
Actually we have a private repository in which not all platform build are there. I was doing a POC to replace poetry with uv for our use case but then we encountered this road block.

---

_Comment by @mmtpo on 2025-08-21 10:38_

Actually, there might be quite a few users hitting this right now. For example, `pandas` currently has a discrepancy between Linux and Windows wheels: Linux/macOS wheels are published in some private mirrors, but Windows wheels lag behind. On Windows, uv resolves the Linux-only version from the first index and then fails, even though PyPI has the Windows-compatible wheels.

```
error: Distribution `pandas==2.3.2 @ registry` can't be installed because it doesn't have a source distribution or wheel for the current platform 
hint: You're on Windows (`win_amd64`), but `pandas` (v2.3.2) only has wheels for the following platforms: `manylinux_2_17_aarch64`, `manylinux_2_17_x86_64`, `manylinux2014_aarch64`, `manylinux2014_x86_64`, `musllinux_1_2_aarch64`, `musllinux_1_2_x86_64`, `macosx_10_13_x86_64`, `macosx_11_0_arm64`; consider adding your platform to `tool.uv.required-environments` to ensure uv resolves to a version with compatible wheels
```

---

_Comment by @konstin on 2025-08-21 10:40_

Why does the private repository mirror the release only partially, instead of mirroring all files of a release?

---

_Referenced in [astral-sh/uv#15547](../../astral-sh/uv/issues/15547.md) on 2025-08-27 19:23_

---

_Comment by @tejal567 on 2025-08-28 06:38_

In our company private repo actually we are only mirroring linux releases and not windows or mac build releases. So when it searches in this private repo and not find it, it raises an error.

---

_Comment by @konstin on 2025-08-28 07:22_

My main recommendation here would be mirroring releases fully, partial releases are difficult for a package manager to deal with in a way that is also secure.

---
