---
number: 7286
title: "uv python list doesn't find pypy"
type: issue
state: closed
author: hynek
labels:
  - bug
  - pypy
  - uv python
assignees: []
created_at: 2024-09-11T10:26:35Z
updated_at: 2024-09-23T22:17:56Z
url: https://github.com/astral-sh/uv/issues/7286
synced_at: 2026-01-10T01:24:12Z
---

# uv python list doesn't find pypy

---

_Issue opened by @hynek on 2024-09-11 10:26_

I find `uv python list` an amazing tool to debug [Python Environment xkcd situations](https://xkcd.com/1987/), even tho I don't use *uv* for downloads. But find my PyPy installation (I could've sworn it used to but I might be wrong).

Here's le output to demonstrate:

```
❯ uv python list
cpython-3.13.0rc2-macos-aarch64-none    /usr/local/bin/python3.13 -> ../../../Library/Frameworks/Python.framework/Versions/3.13/bin/python3.13
cpython-3.13.0rc2-macos-aarch64-none    /Library/Frameworks/Python.framework/Versions/3.13/bin/python3.13
cpython-3.13.0rc2-macos-aarch64-none    /Library/Frameworks/Python.framework/Versions/3.13/bin/python3 -> python3.13
cpython-3.12.6-macos-aarch64-none       /usr/local/bin/python3.12 -> ../../../Library/Frameworks/Python.framework/Versions/3.12/bin/python3.12
cpython-3.12.6-macos-aarch64-none       /usr/local/bin/python3 -> ../../../Library/Frameworks/Python.framework/Versions/3.12/bin/python3
cpython-3.12.6-macos-aarch64-none       /Library/Frameworks/Python.framework/Versions/3.12/bin/python3.12
cpython-3.12.6-macos-aarch64-none       /Library/Frameworks/Python.framework/Versions/3.12/bin/python3 -> python3.12
cpython-3.12.6-macos-aarch64-none       .local/share/uv/python/cpython-3.12.6-macos-aarch64-none/bin/python3 -> python3.12
cpython-3.11.10-macos-aarch64-none      /opt/homebrew/opt/python@3.11/bin/python3.11 -> ../Frameworks/Python.framework/Versions/3.11/bin/python3.11
cpython-3.11.10-macos-aarch64-none      <download available>
cpython-3.11.9-macos-aarch64-none       /usr/local/bin/python3.11 -> ../../../Library/Frameworks/Python.framework/Versions/3.11/bin/python3.11
cpython-3.11.9-macos-aarch64-none       /Library/Frameworks/Python.framework/Versions/3.11/bin/python3.11
cpython-3.11.9-macos-aarch64-none       /Library/Frameworks/Python.framework/Versions/3.11/bin/python3 -> python3.11
cpython-3.10.15-macos-aarch64-none      /opt/homebrew/opt/python@3.10/bin/python3.10 -> ../Frameworks/Python.framework/Versions/3.10/bin/python3.10
cpython-3.10.15-macos-aarch64-none      <download available>
cpython-3.10.11-macos-aarch64-none      /usr/local/bin/python3.10 -> ../../../Library/Frameworks/Python.framework/Versions/3.10/bin/python3.10
cpython-3.9.20-macos-aarch64-none       /opt/homebrew/opt/python@3.9/bin/python3.9 -> ../Frameworks/Python.framework/Versions/3.9/bin/python3.9
cpython-3.9.20-macos-aarch64-none       <download available>
cpython-3.9.6-macos-aarch64-none        /Applications/Xcode.app/Contents/Developer/usr/bin/python3 -> ../../Library/Frameworks/Python3.framework/Versions/3.9/bin/python3
cpython-3.8.20-macos-aarch64-none       /opt/homebrew/opt/python@3.8/bin/python3.8 -> ../Frameworks/Python.framework/Versions/3.8/bin/python3.8
cpython-3.8.20-macos-aarch64-none       <download available>
cpython-3.7.9-macos-x86_64-none         /usr/local/bin/python3.7 -> ../../../Library/Frameworks/Python.framework/Versions/3.7/bin/python3.7
pypy-3.10.14-macos-aarch64-none         <download available>
pypy-3.9.19-macos-aarch64-none          <download available>
pypy-3.8.16-macos-aarch64-none          <download available>

~
❯ type pypy3.10
pypy3.10 is /opt/homebrew/bin/pypy3.10

~
❯ uv --version
uv 0.4.9 (77d278f68 2024-09-10)
```

I can use it to create venvs, though:

```
❯ uv venv --python pypy3.10 foo
Using Python 3.10.14 interpreter at: /opt/homebrew/bin/pypy3.10
Creating virtualenv at: foo
Activate with: source foo/bin/activate.fish
```

---

_Renamed from "uv python list doesn't find pypy anymore" to "uv python list doesn't find pypy" by @hynek on 2024-09-11 10:27_

---

_Label `pypy` added by @AlexWaygood on 2024-09-11 11:11_

---

_Comment by @zanieb on 2024-09-11 13:25_

That's surprising. I'll look into this.

---

_Assigned to @zanieb by @zanieb on 2024-09-11 13:25_

---

_Label `bug` added by @zanieb on 2024-09-11 13:25_

---

_Label `uv python` added by @zanieb on 2024-09-11 13:25_

---

_Comment by @zanieb on 2024-09-12 00:49_

Btw if you don't want to see the downloads you can use `uv python list --only-installed` or set your `python-preference` to `only-system

---

_Comment by @hynek on 2024-09-12 06:12_

> Btw if you don't want to see the downloads you can use `uv python list --only-installed` or set your `python-preference` to `only-system

Ohhh loving it! I didn’t mind the download available but this is really cool.

---

_Comment by @zanieb on 2024-09-12 13:43_

I think this is because, generally, we don't look for `pypy` executables unless you request PyPy so we exclude them here. I'll need to do some refactors to fix this.

---

_Comment by @hynek on 2024-09-12 15:11_

That’s kinda funny in combination with #7118 — sorry for running into all edge cases. 🙈 

---

_Referenced in [astral-sh/uv#7118](../../astral-sh/uv/issues/7118.md) on 2024-09-12 15:17_

---

_Comment by @zanieb on 2024-09-12 15:27_

I appreciate it :)

---

_Comment by @hynek on 2024-09-12 15:41_

![image](https://github.com/user-attachments/assets/a3feb564-c3ce-4236-82c1-94f9a1aa0d78)


---

_Referenced in [astral-sh/uv#7508](../../astral-sh/uv/pulls/7508.md) on 2024-09-18 16:39_

---

_Referenced in [astral-sh/uv#7514](../../astral-sh/uv/pulls/7514.md) on 2024-09-18 18:57_

---

_Referenced in [astral-sh/uv#7649](../../astral-sh/uv/pulls/7649.md) on 2024-09-23 20:48_

---

_Closed by @zanieb on 2024-09-23 22:17_

---
