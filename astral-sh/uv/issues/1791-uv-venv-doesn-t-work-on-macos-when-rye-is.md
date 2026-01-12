```yaml
number: 1791
title: "UV venv doesn't work on MacOS when Rye is installed"
type: issue
state: closed
author: justinhauer
labels:
  - bug
assignees: []
created_at: 2024-02-21T02:32:10Z
updated_at: 2025-11-08T19:25:28Z
url: https://github.com/astral-sh/uv/issues/1791
synced_at: 2026-01-12T15:58:32Z
```

# UV venv doesn't work on MacOS when Rye is installed

---

_@justinhauer_


**Summary:**
Out of the box, the tool doesn't work. 

I have Rye installed in my system. When downloading UV to test this out, as it seems like Rye will soon be dead and I'll have to migrate to this tool. I get the following error: 

**uv platform:** MacOS 14.4 (Intel x86)
**uv version:** uv 0.1.6

**command:** 
` uv venv test`
  × Querying Python at `/Users/justin/.rye/shims/rye` failed with status exit
  │ status: 2:
  │ --- stdout:
 --- stderr:
  │ error: unexpected argument found
  │ ---


It appears that with both tools installed, UV is nerfed.



---

_Label `bug` added by @zanieb on 2024-02-21 03:08_

---

_Comment by @zanieb on 2024-02-21 03:09_

Thanks for the report! Rye won't be dead soon, we'll be helping maintain it.

Regarding the error, it seems weird that we're detecting `rye` as a Python executable. Can you provide output with the `-v` flag?

---

_Comment by @methane on 2024-02-21 06:45_

This is why this error happens.

https://github.com/astral-sh/uv/blob/5d53040465a67bdfdd31512e2eaa0e195909855e/crates/uv-interpreter/src/python_query.rs#L174-L175

```
$ which python3
/Users/inada-n/.rye/shims/python3

$ python3 get_interpreter_info.py
{"markers": ...}

$ ~/.rye/shims/rye get_interpreter_info.py
error: unrecognized subcommand
```

Anyway, rye doesn't provide commands like `python3.12` so we can not `rye venv -p3.12` too.
More effort is needed to support rye managed python interpreters.

---

_Comment by @MichaReiser on 2024-02-21 06:52_

I haven't used rye myself but I wonder if this is the suggested workflow. I think the proposed way of using `uv` with `rye` is to use `uv` through `rye`. I think you can do that by setting `rye config --set-bool behavior.use-uv=true`. See https://lucumr.pocoo.org/2024/2/15/rye-grows-with-uv/

---

_Label `bug` removed by @MichaReiser on 2024-02-21 06:53_

---

_Label `question` added by @MichaReiser on 2024-02-21 06:53_

---

_Comment by @methane on 2024-02-21 06:54_

rye doesn't have `rye venv` command. We can not create venv without project for now.

---

_Comment by @mitsuhiko on 2024-02-21 10:20_

@zanieb i ran into that issue myself. The problem here is that the `python` and `python3` shim by `rye` is detected by name. When `uv` canonicalizes, it tries with the `rye` name instead. For `uv` usage within `rye` that's not an issue because i always bypass the paths where this shows up, but it is an issue if you point it directly to one of the rye shims.

I see three options:

1. pass a hidden env var as part of discovery that can force on the flag in uv
2. always let rye hardlink the shims on all platforms to prevent the symlink resolving
3. to not resolve symlinks in uv

---

_Comment by @methane on 2024-02-21 11:17_

Virtualenv can extend Python discovery and there are extensions for pyenv and rye.

* https://virtualenv.pypa.io/en/latest/extend.html
* https://github.com/un-def/virtualenv-pyenv
* https://github.com/bluss/virtualenv-rye-discovery

Since it needs to be written in Python, difficut to support them from uv.

Another idea is creating directory like what `scripts/bootstrap/install.py` creates.
The directory containing `python3` (default), `python3.X`, and `python3.X.Y` symlinks.
Shell script or Python script is enough for creating such directory for pyenv/rye.

uv already have `UV_TEST_PYTHON_PATH` for finding python from such directory.
By adding the directory to PATH temporary, tox and virtualenv can use it without extension.

---

_Label `question` removed by @MichaReiser on 2024-02-21 11:35_

---

_Label `bug` added by @MichaReiser on 2024-02-21 11:35_

---

_Comment by @MichaReiser on 2024-02-21 11:36_

Possibly related to https://github.com/astral-sh/uv/issues/1795

---

_Comment by @methane on 2024-02-22 09:13_

I find `uv venv -p3.8 spam` doesn't canonicalize symlink.

https://github.com/astral-sh/uv/blob/5d53040465a67bdfdd31512e2eaa0e195909855e/crates/uv-interpreter/src/python_query.rs#L50-L54

Why only find_default_python canonicalize python? Can we just stop canonicalize?

---

_Comment by @zanieb on 2025-11-08 19:25_

We no longer canonicalize the executable path during queries, so I presume this is fixed?

---

_Closed by @zanieb on 2025-11-08 19:25_

---
