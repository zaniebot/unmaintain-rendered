---
number: 11865
title: "Bug Report: uv sync Fails to Install PyQt5 on Windows"
type: issue
state: closed
author: emdezla
labels: []
assignees: []
created_at: 2025-02-28T21:02:59Z
updated_at: 2025-08-21T17:29:16Z
url: https://github.com/astral-sh/uv/issues/11865
synced_at: 2026-01-07T13:12:18-06:00
---

# Bug Report: uv sync Fails to Install PyQt5 on Windows

---

_Issue opened by @emdezla on 2025-02-28 21:02_

### Summary

## **Description**
`uv sync` fails to install `PyQt5-Qt5`, showing an error that no compatible distribution is available for Windows (`win_amd64`). However, `pip install PyQt5` works fine.

## **Steps to Reproduce**
1. Add `PyQt5` dependencies in `pyproject.toml`:
    ```toml
    dependencies = [
        "PyQt5==5.15.11",
        "PyQt5-Qt5==5.15.2",
        "PyQt5-sip==12.17.0"
    ]
    ```
2. Run:
    ```sh
    uv sync
    ```
3. The installation fails with the error:
    ```
    error: Distribution `pyqt5-qt5==5.15.16` can't be installed because it doesn't have a source distribution or wheel for the current platform
    ```

## **Expected Behavior**
`uv sync` should install `PyQt5` properly, like `pip install PyQt5`.

## **Actual Behavior**
`uv` fails, claiming no compatible Windows wheels exist. Moreover, it is picking up a different version that the one described in pyproject.toml. However, `pip install PyQt5` works fine:
```sh
pip install PyQt5
Successfully installed PyQt5-Qt5-5.15.2 PyQt5-sip-12.17.0 PyQt5-5.15.11
 ```

### Platform

win_amd64

### Version

uv 0.5.24

### Python version

3.13.1

---

_Label `bug` added by @emdezla on 2025-02-28 21:02_

---

_Comment by @charliermarsh on 2025-02-28 21:04_

You're 100% sure that your `pyproject.toml` contains `PyQt5-Qt5==5.15.2`? I would be super surprised (and can't reproduce) if we were showing an error about `pyqt5-qt5==5.15.16` in that case.

---

_Label `bug` removed by @charliermarsh on 2025-03-01 00:29_

---

_Label `needs-mre` added by @charliermarsh on 2025-03-01 00:29_

---

_Closed by @charliermarsh on 2025-03-15 01:01_

---

_Comment by @charliermarsh on 2025-03-15 01:02_

Closing due to lack of follow-up, but happy to re-open with an MRE!

---

_Comment by @petebachant on 2025-04-09 16:27_

Encountered a similar issue when running `uv add pyqt5` on Windows. Running `uv venv` and pip installing in there works fine.

<img width="597" alt="Image" src="https://github.com/user-attachments/assets/e12a3851-f7e0-4125-ba1d-cfc1ee294667" />

---

_Comment by @charliermarsh on 2025-04-09 16:28_

Have you read https://docs.astral.sh/uv/concepts/resolution/#required-environments?

---

_Comment by @petebachant on 2025-04-09 17:48_

My project only needs to be able to run on `win32`, so I tried adding that as an environment and required environment, with no change. What did work, however, was to specify the versions that pip solved for:

```
uv add pyqt5==5.15.11 pyqt5-qt5==5.15.2
```

---

_Comment by @jpmorr on 2025-07-31 20:50_

@charliermarsh I think this needs re-opening. qt is a pain and this fails for me:

```powershell
uv add --dev pyqt5
Resolved 110 packages in 1.55s
error: Distribution `pyqt5-qt5==5.15.17 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're on Windows (`win_amd64`), but `pyqt5-qt5` (v5.15.17) only has wheels for the following platforms: `manylinux2014_x86_64`, `macosx_10_13_x86_64`, `macosx_11_0_arm64`; consider adding your platform to `tool.uv.required-environments` to ensure uv resolves to a version with compatible wheels 
```

This is in my `pyproject.toml`:

```toml
[tool.uv]
required-environments = [
    "sys_platform == 'linux'",
    "sys_platform == 'win32'",
]
```

Only by using the specific versions mentioned by @petebachant did I manage to get pyqt5 installed and working. This works:
```powershell
uv add --dev pyqt5==5.15.11 pyqt5-qt5==5.15.2
Resolved 109 packages in 1.31s
Prepared 2 packages in 335ms
Uninstalled 1 package in 5ms
Installed 3 packages in 887ms
 + pyqt5==5.15.11
 + pyqt5-qt5==5.15.2
 + pyqt5-sip==12.17.0

```

I would assume that uv should resolve the correct packages.


---

_Comment by @charliermarsh on 2025-07-31 20:59_

Sorry about that, but I don't think we'll re-open this. It's really a problem with the package itself if they're only publishing wheels for select platforms on select versions. Requiring that all packages have wheels for all platforms would lead to an intractable problem, which is why we require user input to inform of us of the required platforms.


---

_Comment by @jpmorr on 2025-08-01 12:24_

Ok, thanks for the update. `pip` seems to figure this out so I guess in this case I could/should be using `uv pip install`?

---

_Comment by @zanieb on 2025-08-01 13:53_

pip is only performing a resolution for your single platform, while the uv lockfile solves for all platforms so your lockfile is portable across machines. See https://docs.astral.sh/uv/concepts/resolution/#platform-specific-resolution

---

_Label `needs-mre` removed by @konstin on 2025-08-21 17:29_

---

_Referenced in [astral-sh/uv#15421](../../astral-sh/uv/issues/15421.md) on 2025-08-21 17:29_

---

_Referenced in [xiaofeiyu0723/ExVR#40](../../xiaofeiyu0723/ExVR/pulls/40.md) on 2025-10-15 05:53_

---

_Referenced in [neuroinformatics-unit/movement#683](../../neuroinformatics-unit/movement/pulls/683.md) on 2025-10-29 20:15_

---
