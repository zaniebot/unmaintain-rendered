---
number: 14508
title: "Title: uv venv hangs when using managed Python (works with manual python -m venv)"
type: issue
state: open
author: svk1998
labels:
  - question
assignees: []
created_at: 2025-07-08T16:38:31Z
updated_at: 2026-01-04T16:14:06Z
url: https://github.com/astral-sh/uv/issues/14508
synced_at: 2026-01-07T13:12:18-06:00
---

# Title: uv venv hangs when using managed Python (works with manual python -m venv)

---

_Issue opened by @svk1998 on 2025-07-08 16:38_

**Describe the bug**

Running `uv venv` hangs when using the managed Python interpreter installed by `uv`.

This happens even though manually creating a virtual environment using the same interpreter works fine.

---

**To Reproduce**

Steps to reproduce the behavior:

1. Let `uv` install a managed Python:

   ```sh
   uv venv
   ```

2. OR try explicitly passing the Python path:

   ```sh
   uv venv --python "C:\Users\svk19\AppData\Roaming\uv\python\cpython-3.12.11-windows-x86_64-none\python.exe"
   ```

3. Observe that `uv` hangs after logging:

   ```
   TRACE Querying interpreter executable at C:\Users\svk19\AppData\Roaming\uv\python\cpython-3.12.11-windows-x86_64-none\python.exe
   ```

4. However, running:

   ```sh
   C:\Users\svk19\AppData\Roaming\uv\python\cpython-3.12.11-windows-x86_64-none\python.exe -m venv test-env
   ```

   works immediately.

---

**Expected behavior**

`uv venv` should proceed to create the virtual environment instead of hanging.

---

**Environment:**

* OS: Windows 11 (build 22621)
* uv version: 0.7.19
* Installed Python version: cpython-3.12.11 (managed by `uv`)
* Reproduced with both:

  * `uv venv`
  * `uv venv --python "<full path>"`

---

**Additional context**

* `uv self update` reports the latest version (`0.7.19`)
* The hang consistently occurs right after the `TRACE Querying interpreter executable` message
* Disabling `-vvv` doesn't resolve the issue
* Antivirus was checked and ruled out
* Workaround: creating the venv manually and using `uv pip` works fine

---


---

_Comment by @zanieb on 2025-07-08 16:39_

How did you rule out Windows Defender? This is typically a problem with Windows where the system is blocking execution of `python.exe`.

---

_Label `question` added by @zanieb on 2025-07-08 16:39_

---

_Comment by @CHC383 on 2025-07-16 23:33_

Another data point I reported on `mise` side: https://github.com/jdx/mise/discussions/5665, seems to be the same issue that `uv` hangs on using managed Python at `TRACE Querying interpreter executable`.

`mise` internally uses `uv venv` ([code](https://github.com/jdx/mise/blob/e4ea0c3dd916b29a0dc9c8164e5e5de1afef8ae7/src/uv.rs#L60-L64)) to create the virtual env, and the issue happens when mise has automatic virtualenv activation enabled.

Just a high level guess: Is this some soft of recursive bug?
- `uv` tries to create the virtual env and found the `mise` Python shim
- `mise` Python shim tries to resolve to the `uv` venv python (with [uv_venv_auto](https://mise.jdx.dev/mise-cookbook/python.html#mise-uv) enabled), which again triggers `uv venv`
- The steps above repeat

---

_Comment by @wcde on 2026-01-04 10:56_

I have same issue. After creating venv, any commands to uv result in "Querying interpreter executable at", python.exe process starts and completely hangs. Changing the Python version doesn't fix anything.

---

_Comment by @zanieb on 2026-01-04 16:14_

As noted above, this is almost always due to overzealous antivirus on Windows.

---
