---
number: 1346
title: "`uv venv`: macOS, Neither `python` nor `python3` are in `PATH`. Is Python installed?"
type: issue
state: closed
author: Mikcl
labels: []
assignees: []
created_at: 2024-02-15T21:11:22Z
updated_at: 2024-02-15T21:40:10Z
url: https://github.com/astral-sh/uv/issues/1346
synced_at: 2026-01-07T13:12:16-06:00
---

# `uv venv`: macOS, Neither `python` nor `python3` are in `PATH`. Is Python installed?

---

_Issue opened by @Mikcl on 2024-02-15 21:11_

After install `uv` using the macOS installation option, 

The error reports that it is unable to create venv due to python3 not being on $PATH.

```
➜  cd newpip
➜  newpip curl -LsSf https://astral.sh/uv/install.sh | sh
downloading uv 0.1.0 aarch64-apple-darwin
installing to /Users/michaelo/.cargo/bin
  uv
everything's installed!

To add $HOME/.cargo/bin to your PATH, either restart your shell or run:

    source $HOME/.cargo/env
    
➜  newpip source $HOME/.cargo/env
➜  newpip uv --version
uv 0.1.0
➜  newpip which python3
/opt/homebrew/bin/python3
➜  newpip python3 --version
Python 3.11.6
➜  newpip uv venv
  × Neither `python` nor `python3` are in `PATH`. Is Python installed?
```


---

_Comment by @zanieb on 2024-02-15 21:12_

Hi thanks for the issue! Could you share the output of `uv venv -v`?

---

_Comment by @Mikcl on 2024-02-15 21:14_

```
➜  newpip uv venv -v
 uv_interpreter::python_query::find_default_python platform=Platform { os: Macos { major: 14, minor: 2 }, arch: Aarch64 }, cache=Cache { root: "/Users/michaelo/Library/Caches/uv", refresh: None, _temp_dir_drop: None }
  × Neither `python` nor `python3` are in `PATH`. Is Python installed?
```

---

_Comment by @zanieb on 2024-02-15 21:14_

Does it work when installed via `pip install uv` instead?

https://github.com/astral-sh/uv/issues/1341#issuecomment-1947347813

---

_Renamed from "`uv venv`: Neither `python` nor `python3` are in `PATH`. Is Python installed?" to "`uv venv`: macOS, Neither `python` nor `python3` are in `PATH`. Is Python installed?" by @Mikcl on 2024-02-15 21:17_

---

_Comment by @Mikcl on 2024-02-15 21:19_

no it doesnt, but  the following does.
```
python3 -m venv venv
source venv/bin/activate
python -m pip install uv
uv venv
```

---

_Comment by @zanieb on 2024-02-15 21:30_

Should be closed by https://github.com/astral-sh/uv/pull/1351

---

_Comment by @Mikcl on 2024-02-15 21:32_

that should indeed thanks :D 

---

_Comment by @zanieb on 2024-02-15 21:39_

Thanks for the quick feedback! Sorry it wasn't working.

---

_Closed by @zanieb on 2024-02-15 21:39_

---

_Comment by @zanieb on 2024-02-15 21:40_

v0.1.1 is on its way out

---
