```yaml
number: 9961
title: Build from source packages
type: issue
state: closed
author: hoegge
labels:
  - question
assignees: []
created_at: 2024-12-17T07:40:39Z
updated_at: 2024-12-17T15:25:30Z
url: https://github.com/astral-sh/uv/issues/9961
synced_at: 2026-01-12T16:00:03Z
```

# Build from source packages

---

_@hoegge_


# Feature Request: Add fallback to `pip` for building/installing packages from source in `uv sync`

## Description
`uv` is an amazing solution that significantly lowers entry barriers to Python development and package management. However, I have encountered scenarios where `uv sync` cannot install packages that require building from source (e.g., when no binary wheels are available).

Currently, I need to manually install these packages using:

```bash
uv pip install <package>
```

This workaround works but introduces a manual step, making the workflow less seamless.

## Feature Request
Would it be possible for `uv sync` to **fall back to `pip`** (or an equivalent mechanism) when no binary wheels are available? For example:
1. Attempt to install the package via binary wheel.
2. If no wheel is available, invoke `pip` (or build tools) to build the package from source.

## Benefits
- Simplifies workflows for projects relying on packages that only distribute source code.
- Allows `uv` to remain the single command for dependency management.
- Improves compatibility with projects requiring source builds.

---

Thank you for considering this feature request! I’d be happy to provide more details or feedback if needed.


---

_Comment by @FishAlchemist on 2024-12-17 09:58_

Since ``uv pip install`` works, ``uv sync`` should work as well, right? Do you have any examples where it doesn't succeed?

---

_Comment by @hoegge on 2024-12-17 12:24_

E.g. if I try to install pyside6 I get:

```
PS C:\Users\mhp\Development\QtTest> uv add pyside pyside6
Using CPython 3.13.1
Creating virtual environment at: .venv
  × Failed to download and build `pyside==1.2.4`
  ╰─▶ Build backend failed to determine requirements with `build_wheel()` (exit code: 1)

      [stdout]
      only these python versions are supported: [(2, 6), (2, 7), (3, 2), (3, 3), (3, 4)]

      [stderr]
      <string>:1: SyntaxWarning: invalid escape sequence '\Q'
        import sys
      <string>:117: SyntaxWarning: invalid escape sequence '\d'
      C:\Users\mhp\AppData\Local\uv\cache\sdists-v6\pypi\pyside\1.2.4\8zHWHenJ4BGIM20N9sOlu\src\utils.py:501: SyntaxWarning: invalid escape sequence '\d'     
        '[\d.]+\)')

  help: `pyside` (v1.2.4) was included because `qttest` (v0.1.0) depends on `pyside`
```
with `uv add` and:
```
PS C:\Users\mhp\Development\QtTest> uv pip install pyside6
Resolved 4 packages in 263ms
Installed 4 packages in 1.41s
 + pyside6==6.8.1
 + pyside6-addons==6.8.1
 + pyside6-essentials==6.8.1
 + shiboken6==6.8.1
PS C:\Users\mhp\Development\QtTest>
```
with `uv pip install`

---

_Comment by @FishAlchemist on 2024-12-17 13:06_

@hoegge What's the Python version of the virtual environment you installed it in? Or, provide the --verbose output, it should be written there as well.
But the error message says PySide doesn't play nice with Python 3.5 and above. So, it's no surprise that the build failed using a newer Python version.
Actually, PySide seems to have a newer package called PySide6,  So if you don't have any specific requirements for older Python versions, you can just install the newer one.



---

_Comment by @hoegge on 2024-12-17 13:50_

@FishAlchemist Ah - can side the example was bad due to a type adding both pyside and pyside6.

My first problem was installing pandasgui:

```
PS C:\Users\mhp\uvinstalltest> uv add pandasgui
Resolved 57 packages in 574ms
error: Distribution `pyqt5-qt5==5.15.15 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution 
or wheel for the current platform


PS C:\Users\mhp\uvinstalltest> uv pip install pandasgui
Resolved 45 packages in 285ms
   Built pandasgui==0.2.14
   Built qtstylish==0.1.5
Prepared 10 packages in 3.83s
Installed 44 packages in 12.13s
```

Where UV could not install but pip could.

---

_Comment by @zanieb on 2024-12-17 15:25_

`uv pip` and `uv sync` are performing fundamentally different resolutions. The difference is not that `uv pip` succeeds in building a distribution that `uv sync` cannot, they're building totally different versions, e.g., `Failed to download and build pyside==1.2.4` vs `Installed... pyside6==6.8.1`.

Please read some more about the resolver in https://docs.astral.sh/uv/concepts/resolution/ — we can't just fall back to installing for a single platform. Instead, we're making the resolver robust to these problems.

See also

- https://github.com/astral-sh/uv/issues/9711
- https://github.com/astral-sh/uv/pull/9827

If you need help with a particular failing resolution, feel free to open a new issue with the details necessary to reproduce it.

---

_Closed by @zanieb on 2024-12-17 15:25_

---

_Label `question` added by @zanieb on 2024-12-17 15:25_

---
