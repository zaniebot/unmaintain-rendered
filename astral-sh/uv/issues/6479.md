```yaml
number: 6479
title: "Should `uv python list` show `pyenv` versions as well?"
type: issue
state: closed
author: jimmcslim
labels:
  - question
assignees: []
created_at: 2024-08-23T00:34:57Z
updated_at: 2025-02-01T05:38:25Z
url: https://github.com/astral-sh/uv/issues/6479
synced_at: 2026-01-10T03:50:30Z
```

# Should `uv python list` show `pyenv` versions as well?

---

_Issue opened by @jimmcslim on 2024-08-23 00:34_

I'm using `uv 0.3.1 (Homebrew 2024-08-21)`

With the references to `pyenv` in the `uv` documentation it is unclear to me if I should also be seeing the list of Python's that `pyenv` has installed.

From `uv python list` I get...

```
cpython-3.12.5-macos-aarch64-none     /opt/homebrew/opt/python@3.12/bin/python3.12 -> ../Frameworks/Python.framework/Versions/3.12/bin/python3.12
cpython-3.12.5-macos-aarch64-none     <download available>
cpython-3.11.9-macos-aarch64-none     <download available>
cpython-3.10.14-macos-aarch64-none    <download available>
cpython-3.9.19-macos-aarch64-none     /opt/homebrew/opt/python@3.9/bin/python3.9 -> ../Frameworks/Python.framework/Versions/3.9/bin/python3.9
cpython-3.9.19-macos-aarch64-none     <download available>
cpython-3.9.6-macos-aarch64-none      /Applications/Xcode.app/Contents/Developer/usr/bin/python3 -> ../../Library/Frameworks/Python3.framework/Versions/3.9/bin/python3
cpython-3.8.19-macos-aarch64-none     <download available>
```

From `pyenv versions` I get...

```
* system (set by /Users/jim/.pyenv/version)
  2.7.18
  3.8.12
  3.9.4
  3.10.2
  3.10.3
  3.11.8
  3.12.4
```

Running `python3` and querying `us.path.dirname(sys.executable)` in the REPL I get `/opt/homebrew/opt/python@3.12/bin`

If I run `pyenv local 3.9.4` and then run `uv python list` I get...

```
cpython-3.12.5-macos-aarch64-none     /opt/homebrew/opt/python@3.12/bin/python3.12 -> ../Frameworks/Python.framework/Versions/3.12/bin/python3.12
cpython-3.12.5-macos-aarch64-none     <download available>
cpython-3.11.9-macos-aarch64-none     <download available>
cpython-3.10.14-macos-aarch64-none    <download available>
cpython-3.9.19-macos-aarch64-none     /opt/homebrew/opt/python@3.9/bin/python3.9 -> ../Frameworks/Python.framework/Versions/3.9/bin/python3.9
cpython-3.9.19-macos-aarch64-none     <download available>
cpython-3.9.6-macos-aarch64-none      /Applications/Xcode.app/Contents/Developer/usr/bin/python3 -> ../../Library/Frameworks/Python3.framework/Versions/3.9/bin/python3
cpython-3.9.4-macos-aarch64-none      /Users/jim/.pyenv/versions/3.9.4/bin/python3.9
cpython-3.9.4-macos-aarch64-none      /Users/jim/.pyenv/versions/3.9.4/bin/python3 -> python3.9
cpython-3.9.4-macos-aarch64-none      /Users/jim/.pyenv/versions/3.9.4/bin/python -> python3.9
cpython-3.8.19-macos-aarch64-none     <download available>
```

Which shows that having specified a version of Python in a local `.python-version` file it finds the `pyenv` managed version.

Just not sure if I should be expecting to see ALL of the pyenv versions in the `uv python list` - and if not, might be worth updating the Discovery documentation to make it clear that `uv` will only have visibility of the currently activated `pyenv` Python.

---

_Comment by @zanieb on 2024-08-23 12:39_

If `pyenv` puts executables on the PATH, we'll find them, but we don't support querying pyenv-specific locations or pyenv itself. Details on what we support at https://docs.astral.sh/uv/concepts/python-versions/#discovery-of-python-versions

---

_Label `question` added by @zanieb on 2024-08-23 12:39_

---

_Closed by @zanieb on 2024-10-21 21:18_

---

_Comment by @XiLaiTL on 2025-01-29 01:21_

@zanieb I just set the `UV_PYTHON_INSTALL_DIR` to the folder of pyenv's versions. But I cannot get the python downloaded by pyenv in `uv python list`.

Why?

---

_Comment by @zanieb on 2025-01-29 14:32_

`UV_PYTHON_INSTALL_DIR` is where we install Python versions — we don't know how to read pyenv's structure.

---

_Comment by @XiLaiTL on 2025-02-01 03:05_

@zanieb I wonder how uv read the python structure in `UV_PYTHON_INSTALL_DIR`.

If it just read by matching the folder name like `cpython-3.10.14-windows-x86_64-none`, I think it is easy to read the folder matched by the pattern `3.X.X` which means version, downloading by pyenv. 

If it read more file in folder, I wonder which files will be regarded as a valid python interpreter.

---

_Comment by @zanieb on 2025-02-01 05:38_

We're not willing to read folder structures defined by other tools. I'd recommend placing those directories on the `PATH` if you want us to discover interpreters in them.

---
