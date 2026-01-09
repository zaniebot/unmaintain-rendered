---
number: 7320
title: "Minor: `uv tool install` should be able to modify which Python a tool uses."
type: issue
state: closed
author: ilyagr
labels:
  - bug
  - uv tool
assignees: []
created_at: 2024-09-12T01:54:16Z
updated_at: 2024-09-18T06:37:43Z
url: https://github.com/astral-sh/uv/issues/7320
synced_at: 2026-01-07T13:12:17-06:00
---

# Minor: `uv tool install` should be able to modify which Python a tool uses.

---

_Issue opened by @ilyagr on 2024-09-12 01:54_

This is loosely related to #6297 (**Update** and/or https://github.com/astral-sh/uv/issues/7287). I have `poetry` installed with the default option, which meant that it used Homebrew python. I wanted to switch to managed python.

```console
$ pwd
/Users/ilyagr/.local/share/uv/tools/poetry/bin
$ realpath python
/opt/homebrew/Cellar/python@3.12/3.12.6/Frameworks/Python.framework/Versions/3.12/bin/python3.12
$ uv tool install poetry --python-preference only-managed --reinstall
<Installs managed python>
<Some output omitted>
Installed 1 executable: poetry
$ realpath python # Should have changed but didn't
/opt/homebrew/Cellar/python@3.12/3.12.6/Frameworks/Python.framework/Versions/3.12/bin/python3.12
```

After `uv tool uninstall poetry`, `uv tool install poetry --python-preference only-managed` did work correctly.

```console
$ realpath python
/Users/ilyagr/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/bin/python3.12
```

Either the original command should have changed `poetry` to use the correct python version, or it should have shown an error and *should not* have installed the managed python. In the latter case, it could suggest `uv tool upgrade --python` once #6297 is solved.

I'm using `uv 0.4.9 (77d278f68 2024-09-10)` on ARM Mac OS X.

---

_Renamed from "`uv tool install` should be able to modify which Python a tool uses." to "Minor: `uv tool install` should be able to modify which Python a tool uses." by @ilyagr on 2024-09-12 01:54_

---

_Comment by @zanieb on 2024-09-12 02:17_

This sounds like a bug, thanks for the report!

---

_Label `bug` added by @zanieb on 2024-09-12 02:17_

---

_Label `uv tool` added by @zanieb on 2024-09-12 02:17_

---

_Comment by @zanieb on 2024-09-12 02:17_

cc @charliermarsh the solution may be immediately obvious to you here

---

_Comment by @charliermarsh on 2024-09-14 02:57_

We fetch a matching Python interpreter upfront because we (might) need it to resolve unnamed requirements. But we then reuse the existing environment later when reinstalling the package.

I am wondering if `--reinstall` should never reuse the existing environment.


---

_Comment by @zanieb on 2024-09-14 03:04_

I think even without `--reinstall` if we select a different Python interpreter than the one in the environment we should replace it?

---

_Comment by @charliermarsh on 2024-09-14 03:16_

Do you think "any different Python interpreter"? Or just that we should take `--python-preference` into account when determining whether the existing interpreter satisfies the request (like how we would allow a Python 3.12.3 interpreter today if the user passed `--python 3.12`, even if Python 3.12.4 was also installed)?

---

_Comment by @charliermarsh on 2024-09-14 03:19_

(The former is way easier.)

---

_Comment by @zanieb on 2024-09-16 01:05_

I think "any different Python interpreter" sounds compelling though it seems a little dubious to do a reinstall when `--python 3.12` is requested and there's just a different patch version. Hm. Idk it still seems simple to teach that if we resolve to a different interpreter we'll use it on `install`.

---

_Comment by @lucab on 2024-09-16 18:48_

I started looking into this and noticed that there is already some logic wired to the `--python` flag, which can (conservatively) decide to invalidate an environment:
https://github.com/astral-sh/uv/blob/23494d85ab7d416e7f746b28b1d5897e16b1b9ff/crates/uv/src/commands/tool/install.rs#L275-L292

If I understood the CLI logic correctly, it feels like the issue can be scoped back to "install-or-upgrade without explicit -p".

In that case, we may want to enhance the fallback case (hardcoded `true` in the logic above) to something smarter like:
```rust
settings.reinstall.is_none() ||
settings.upgrade.is_none() ||
(environment.interpreter().markers() == interpreter.markers())
```

I haven't seen better primitives to implement a more conservative comparison logic without comparing markers (e.g. based on a managed/system bit).
The heuristic above may be a bit overkill, but should definitely cover all possible cases.

---

_Comment by @charliermarsh on 2024-09-16 18:51_

> environment.interpreter().markers() == interpreter.markers()

I think this wouldn't be quite enough, since the example use-case here is about managed vs. unmanaged Pythons which isn't captured in the markers. ("Managed" means the interpreter was installed by uv via `uv python install`.)

I think we have two options: (1) _always_ reset if the interpreter is different; or (2) upgrade `python_request.satisfied` to take the "preference" into account, such that we invalidate the environment if the user passes `--python-preference only-managed` but we used an unmanaged Python.

(2) is harder -- we don't really track whether the current Python is managed IIUC.


---

_Comment by @zanieb on 2024-09-16 18:52_

Seems easiest to just do (1) since it's already a new "install" invocation. If it was a more frequent operation, I'd be more concerned about invalidating too eagerly.

---

_Comment by @lucab on 2024-09-16 19:15_

Makes sense, thanks for the feedback.

The whole `Interpreter` itself isn't `Eq` right now, but I think it can be made so if needed.
Is the binary path (`sys_executable`) enough to compare to detect if it is the same interpreter (I think so), or is there some edge behavior that requires deep-comparing all of the fields below?
https://github.com/astral-sh/uv/blob/23494d85ab7d416e7f746b28b1d5897e16b1b9ff/crates/uv-python/src/interpreter.rs#L30-L50

---

_Comment by @zanieb on 2024-09-16 20:06_

For this purpose it seems fine to compare `sys_executable`. I'm not sure about a general `Eq` implementation, but @konstin would probably know.

---

_Comment by @charliermarsh on 2024-09-16 20:09_

Yeah I think you basically want `is_same_executable` from the `satisfied` check.

---

_Referenced in [astral-sh/uv#7451](../../astral-sh/uv/pulls/7451.md) on 2024-09-17 08:20_

---

_Referenced in [astral-sh/uv#7471](../../astral-sh/uv/issues/7471.md) on 2024-09-17 16:51_

---

_Closed by @lucab on 2024-09-18 06:37_

---

_Closed by @lucab on 2024-09-18 06:37_

---
