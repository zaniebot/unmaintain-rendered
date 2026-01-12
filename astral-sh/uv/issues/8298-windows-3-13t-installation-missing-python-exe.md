```yaml
number: 8298
title: Windows 3.13t installation missing python.exe
type: issue
state: closed
author: colesbury
labels:
  - bug
  - uv python
assignees: []
created_at: 2024-10-17T17:47:21Z
updated_at: 2024-12-06T04:24:45Z
url: https://github.com/astral-sh/uv/issues/8298
synced_at: 2026-01-12T15:59:23Z
```

# Windows 3.13t installation missing python.exe

---

_@colesbury_

```shell
> uv run -p 3.13t python
error: Python interpreter not found at `C:\Users\Administrator\AppData\Roaming\uv\python\cpython-3.13.0+freethreaded-windows-x86_64-none\python.exe`
```

The installation directory has a `python3.13t.exe`, but no `python.exe`

This also affects other commands like `uv venv`.

---

_Comment by @zanieb on 2024-10-17 18:28_

https://github.com/python/cpython/issues/112984#issue-2036766508 says

> The installer should not include a python.exe, to avoid causing PATH conflicts, but Nuget, embeddable and Store packages should include a copy/alias.

so I guess we should add one in our installs? I wonder if we should do that upstream or here.

---

_Label `uv python` added by @zanieb on 2024-10-17 18:28_

---

_Comment by @colesbury on 2024-10-17 18:49_

My suggestion would be to rename the `python3.13t.exe` to `python.exe` in the [indygreg/python-build-standalone](
https://github.com/indygreg/python-build-standalone/blob/9780ebb124f795f8976d7924b9df505143d04f25/cpython-windows/build.py#L1451-L1455) build process, especially because the `python-build-standalone` Windows executables don't otherwise include the Python version numbers in their names.



---

_Comment by @zanieb on 2024-10-17 18:58_

> especially because the python-build-standalone Windows executables don't otherwise include the Python version numbers in their names.

As far as I know, we're just following the standard Windows build behavior there (not including version numbers), and it's proper for us to continue doing so for `python3.13t.exe` (which is the canonical name CPython uses here)

Since they suggest creating a copy, I think that's what I'll do? Exploring in https://github.com/indygreg/python-build-standalone/pull/373

---

_Label `bug` added by @zanieb on 2024-10-17 19:01_

---

_Comment by @zanieb on 2024-10-17 19:02_

cc @zooba if you care to weigh in on what is best here.

---

_Comment by @colesbury on 2024-10-17 19:10_

As a overall suggestion: I think you should be aiming for cross-platform consistency -- `uv` is a cross-platform product -- rather than consistency with parts of the CPython Windows installer (or macOS installer for that matter). People will expect that `uv` commands that work on Linux and macOS to also work on Windows.

For example, I think all of the following commands should work on all platforms:

```
uv run -p 3.10 python
uv run -p 3.10 python3.10

uv run -p 3.13t python
uv run -p 3.13t python3.13
uv run -p 3.13t python3.13t
```

---

_Comment by @zooba on 2024-10-17 19:22_

Yeah, I largely agree with Sam here, cross-platform consistency is for the best. We only don't do that in the Windows installer because it's a change that will definitely break people, and I don't want to upset people more until we've actually got a sustainable path forward. `uv` is much closer to that vision than `PATH` modifications will ever be.

---

_Comment by @zanieb on 2024-10-17 19:46_

Great thanks for the input! I'll rename it upstream — makes life a bit easier anyway.

I'm not sure what the best approach will be to address your `uv run` examples, but generally agree with the philosophy that the commands should be portable across platforms.

Thanks again @colesbury for following up on our handling of these distributions, there are definitely some quirks to be ironed out.

---

_Comment by @colesbury on 2024-10-21 15:29_

@zanieb - running `uv run -p 3.13t python` works for me today on Windows, but I see that your PR to `python-build-standalone` has not landed, so I'm not sure what changed.

I've run into some other confusing behavior where `uv python list` temporarily did not show `3.13t` as installed, but `uv python install 3.13t` reported it as installed. I'm unable to reliably reproduce the behavior.  

```
C:\Users\Administrator>uv python install 3.13t
Searching for Python versions matching: Python 3.13t
Found existing installation for Python 3.13t: cpython-3.13.0+freethreaded-windows-x86_64-none

C:\Users\Administrator>uv python list
cpython-3.13.0-windows-x86_64-none     <download available>
cpython-3.12.7-windows-x86_64-none     <download available>
cpython-3.11.10-windows-x86_64-none    <download available>
cpython-3.10.15-windows-x86_64-none    <download available>
cpython-3.9.20-windows-x86_64-none     <download available>
cpython-3.8.20-windows-x86_64-none     <download available>
cpython-3.7.9-windows-x86_64-none      <download available>
pypy-3.10.14-windows-x86_64-none       <download available>
pypy-3.9.19-windows-x86_64-none        <download available>
pypy-3.8.16-windows-x86_64-none        <download available>
pypy-3.7.13-windows-x86_64-none        <download available>

C:\Users\Administrator>uv python install 3.13t
Searching for Python versions matching: Python 3.13t
Found existing installation for Python 3.13t: cpython-3.13.0+freethreaded-windows-x86_64-none

C:\Users\Administrator>uv run -p 3.13t python
Python 3.13.0 experimental free-threading build (main, Oct 16 2024, 00:29:34) [MSC v.1929 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> quit()

C:\Users\Administrator>uv python list
cpython-3.13.0+freethreaded-windows-x86_64-none    AppData\Roaming\uv\python\cpython-3.13.0+freethreaded-windows-x86_64-none\python.exe
cpython-3.12.7-windows-x86_64-none                 <download available>
cpython-3.11.10-windows-x86_64-none                <download available>
cpython-3.10.15-windows-x86_64-none                <download available>
cpython-3.9.20-windows-x86_64-none                 <download available>
cpython-3.8.20-windows-x86_64-none                 <download available>
cpython-3.7.9-windows-x86_64-none                  <download available>
pypy-3.10.14-windows-x86_64-none                   <download available>
pypy-3.9.19-windows-x86_64-none                    <download available>
pypy-3.8.16-windows-x86_64-none                    <download available>
pypy-3.7.13-windows-x86_64-none                    <download available>
```

---

_Comment by @zanieb on 2024-10-21 16:03_

@colesbury I added https://github.com/astral-sh/uv/pull/8310 as a temporary workaround in uv — just much easier to land a quick change here than upstream.

I'll try to look into that `uv python list` behavior, sounds pretty weird. If you can get `RUST_LOG=uv=trace uv python list -v` logs on the failure that'd be helpful.

---

_Comment by @zanieb on 2024-10-31 14:40_

Will be fixed in the next upstream release too

---

_Closed by @zanieb on 2024-10-31 14:40_

---

_Comment by @zanieb on 2024-12-06 04:24_

Tragically, changing the path upstream post-build breaks virtual environments. See https://github.com/indygreg/python-build-standalone/issues/405

Any insights would be appreciated!

---
