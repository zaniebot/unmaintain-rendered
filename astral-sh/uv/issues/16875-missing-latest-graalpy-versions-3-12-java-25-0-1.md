```yaml
number: 16875
title: Missing Latest GraalPy Versions (3.12, Java 25.0.1) in uv
type: issue
state: closed
author: ezzarghili
labels:
  - question
assignees: []
created_at: 2025-11-27T14:11:31Z
updated_at: 2025-11-30T15:13:29Z
url: https://github.com/astral-sh/uv/issues/16875
synced_at: 2026-01-12T16:02:39Z
```

# Missing Latest GraalPy Versions (3.12, Java 25.0.1) in uv

---

_@ezzarghili_

### Question

When running `uv python install graalpy`, only GraalPy 3.11 is available for download. The latest GraalPy releases (such as 3.12 with Java 25.0.1) are not downloadable.

**Steps to Reproduce:**

1. Run:  
```
uv python install graalpy
```
2. Observe the available versions.

Expected Behavior: 
The latest GraalPy versions, including 3.12 (Java 25.0.1), should be available for installation through uv.

Actual Behavior: 
version 3.11 is offered:
```
$ uv python install graalpy                                         
Installed Python 3.11.0 in 26.50s
 + graalpy-3.11.0-macos-aarch64-none (python3.11)
$ /Users/REDACTED/.local/bin/python3.11
Python 3.11.7 (Wed Jul 02 10:42:43 PDT 2025)
[Graal, Oracle GraalVM, Java 24.0.2 (aarch64)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
```

**Additional Context:**  
I am one of the maintainers of the GraalVM/GraalPy projects, if anything is needed to make this happen from our side please let me know.

**Environment:**
- OS: macOS (Apple Silicon)

Thank you!

### Platform

macOS (Apple Silicon) 15.6

### Version

uv 0.8.9

---

_Label `question` added by @ezzarghili on 2025-11-27 14:11_

---

_Comment by @my1e5 on 2025-11-27 14:47_

> Version
> uv 0.8.9

I think you just need to update your uv installation. GraalPy 3.12 support was added in uv version 0.8.18:
- https://github.com/astral-sh/uv/pull/15900

and 25.0.1 was released in 0.9.6

- https://github.com/astral-sh/uv/pull/16401

---

_Closed by @zanieb on 2025-11-30 15:13_

---
