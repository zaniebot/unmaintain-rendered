```yaml
number: 2368
title: "[Infinite loop] When fixing empty __init__ file."
type: issue
state: closed
author: Alphasite
labels: []
assignees: []
created_at: 2023-01-30T22:50:35Z
updated_at: 2023-01-30T23:01:39Z
url: https://github.com/astral-sh/ruff/issues/2368
synced_at: 2026-01-12T15:54:42Z
```

# [Infinite loop] When fixing empty __init__ file.

---

_@Alphasite_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->

```
❯ ruff --isolated --fix shepherd_public_api/__init__.py 

shepherd2/server/api-public on  main [$!+?] is 📦 v0.1.0 via 🐍 v3.10.8 (shepherd-reconciliation-zyKwjI-E-py3.11) on ☁️  (us-west-1) on ☁️  nishadm@vmware.com(us-central1) 
❯ ruff --isolated --fix shepherd_public_api/__init__.py --select D
warning: `D203` (OneBlankLineBeforeClass) and `D211` (NoBlankLinesBeforeClass) are incompatible. Consider adding `D203` to `ignore`.

warning: Failed to converge after 100 iterations.

This likely indicates a bug in `ruff`. If you could open an issue at:

https://github.com/charliermarsh/ruff/issues/new?title=%5BInfinite%20loop%5D

quoting the contents of `shepherd_public_api/__init__.py`, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

shepherd_public_api/__init__.py:1:1: D213 Multi-line docstring summary should start at the second line
shepherd_public_api/__init__.py:1:1: D404 First word of the docstring should not be "This"
Found 102 errors (100 fixed, 2 remaining).
```

File contents:
```
"""This package has the public REST Api implementation.

The apis here are public, documented and versioned. The core
framework is Starlite.

...
"""
```

We hit around 14 infinite loops in the project, are you interested in some of the others? I'm not what i can share without causing issues.

---

_Renamed from "[Infinite loop]" to "[Infinite loop] When fixing empty __init__ file." by @Alphasite on 2023-01-30 22:51_

---

_Comment by @charliermarsh on 2023-01-30 22:51_

Can you try disabling one of `D212` or `D213`? Those conflict, similarly to `D203` and `D211`.

---

_Comment by @charliermarsh on 2023-01-30 22:52_

(We're either going to remove those conflicting rules, or make it harder to enable them (#2289, #2292).)

---

_Comment by @charliermarsh on 2023-01-30 22:52_

(Based on your example, you want to disable `D213`.)

---

_Comment by @Alphasite on 2023-01-30 22:59_

Ah that did it and resolved all of the conflicts, and wow thank you for your incredibly fast response! Do you want me to close out this issue?

---

_Comment by @charliermarsh on 2023-01-30 23:01_

No worries. I'm really sorry you ran into this. It's like, the biggest footgun in all of Ruff right now. I really want to fix it in the next release!

I'm gonna close since we're already tracking elsewhere. Please keep filing if you run into more issues.

---

_Closed by @charliermarsh on 2023-01-30 23:01_

---
