```yaml
number: 1640
title: "Virtual environments break on Homebrew upgrades due to using a `Cellar` link"
type: issue
state: closed
author: cjolowicz
labels:
  - bug
  - macos
  - great writeup
  - virtualenv
assignees: []
created_at: 2024-02-18T11:49:01Z
updated_at: 2024-12-10T20:42:11Z
url: https://github.com/astral-sh/uv/issues/1640
synced_at: 2026-01-10T04:36:19Z
```

# Virtual environments break on Homebrew upgrades due to using a `Cellar` link

---

_Issue opened by @cjolowicz on 2024-02-18 11:49_

On Homebrew, virtual environments created by `uv venv` reference the Python installation under `Cellar` in their interpreter symlink and `pyvenv.cfg`, which has the full downstream version in its path. These virtual environments break when Homebrew upgrades the respective Python package to the next maintenance release. In recent versions of `venv` and `virtualenv`, this issue was resolved by using the stable link under `$(brew --prefix)/opt/python@3.x/` instead. For example, on Python 3.11 macOS aarch64 this would be the interpreter in `/opt/homebrew/opt/python@3.11/bin`.

This shell session demonstrates the problem:

```sh
❯ uv venv
❯ readlink .venv/bin/python
/opt/homebrew/Cellar/python@3.11/3.11.7_1/Frameworks/Python.framework/Versions/3.11/bin/python3.11
❯ grep ^home .venv/pyvenv.cfg
home = /opt/homebrew/Cellar/python@3.11/3.11.7_1/Frameworks/Python.framework/Versions/3.11/bin
```

When Homebrew upgrades its `python@3.11` package and cleans up the old installation under `Cellar`, those references in the virtual environment start to dangle.

For comparison, here's what I get with `venv` from Homebrew's `python@3.11` installation:

```sh
❯ python3.11 -m venv .venv
❯ readlink .venv/bin/python
python3.11
❯ readlink .venv/bin/python3.11
/opt/homebrew/opt/python@3.11/bin/python3.11
❯ grep ^home .venv/pyvenv.cfg
home = /opt/homebrew/opt/python@3.11/bin
```

And with `virtualenv`:

```sh
❯ virtualenv --version
virtualenv 20.25.0 from ~/.local/pipx/venvs/virtualenv/lib/python3.12/site-packages/virtualenv/__init__.py
❯ virtualenv .venv
❯ readlink .venv/bin/python
/opt/homebrew/opt/python@3.12/bin/python3.12
❯ grep ^home .venv/pyvenv.cfg
home = /opt/homebrew/opt/python@3.12/bin
```

Affected platforms: Linux and macOS with Homebrew Python

```
❯ uv --version
uv 0.1.4
❯ gcc -dumpmachine
arm64-apple-darwin23.2.0
``` 

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Label `bug` added by @MichaReiser on 2024-02-18 12:04_

---

_Label `macos` added by @MichaReiser on 2024-02-18 12:04_

---

_Label `virtualenv` added by @zanieb on 2024-02-18 20:37_

---

_Label `great writeup` added by @zanieb on 2024-02-18 21:13_

---

_Comment by @charliermarsh on 2024-03-01 19:45_

Do you by any chance have a link to the relevant PRs or issues in `venv` and/or `virtualenv`?

---

_Comment by @cjolowicz on 2024-03-02 00:44_

I don't, but FWIW `venv` derives the `home` key from `sys._base_executable`, which is the stable path.

---

_Comment by @charliermarsh on 2024-03-02 01:01_

Hmm yeah, we're using `sys._base_executable` as of an open PR, but even that gives me:

```shell
❯ python3.8
Python 3.8.18 (default, Aug 24 2023, 19:48:18)
[Clang 15.0.0 (clang-1500.1.0.2.5)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> sys._base_executable
'/opt/homebrew/bin/python3.8'
```

---

_Comment by @charliermarsh on 2024-03-02 02:06_

The problem is that we're calling `canonicalize` on the `sys.executable`.

---

_Comment by @charliermarsh on 2024-03-02 02:16_

@konstin -- We may want to revisit this change (https://github.com/astral-sh/uv/issues/965). I think we should only do this (`canonicalize`) if we're in a virtual environment?

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-02 02:16_

---

_Comment by @charliermarsh on 2024-03-02 02:23_

We may want something like...

- If we're not in a venv, use `sys.executable`.
- If we are in a venv, resolve symlinks once (not recursively) on `sys.executable`.

That's closer to virtualenv:

```python
# if we're not in a virtual environment, this is already a system python, so return the original executable
# note we must choose the original and not the pure executable as shim scripts might throw us off
return self.original_executable
```


---

_Comment by @ofek on 2024-03-02 02:35_

I don't know if it's quite relevant but they just fixed a bug by storing the resolved absolute path in the metadata file that is generated for virtual environments: https://github.com/pypa/virtualenv/issues/2682

Also, I know all of you are basically Rust experts, but in case I am reading the source correctly please never ever use canonicalize directly as it is literally (I'm not joking) [broken on Windows](https://github.com/rust-lang/rust/issues/42869) and will cause all of us bugs and unexpected behavior. Please don't use it. Instead, it's common to use the [dunce](https://crates.io/crates/dunce) crate or the [normpath](https://crates.io/crates/normpath) crate when you don't want to resolve symlinks. See even [Armin talking about it](https://twitter.com/mitsuhiko/status/1699381180536086714).

---

_Comment by @charliermarsh on 2024-03-02 02:54_

Haha. We do use `fs::canonicalize` but we then strip UNC paths everywhere. (I guess we could use dunce's `canonicalize` to simplify that process.)

---

_Comment by @charliermarsh on 2024-03-02 02:57_

I'd actually expect that virtualenv change to _break_ this case, if I'm reading it correctly:
```
❯ realpath /opt/homebrew/opt/python@3.8/bin/python3.8
/opt/homebrew/Cellar/python@3.8/3.8.18_2/Frameworks/Python.framework/Versions/3.8/bin/python3.8
```

So now `virtualenv` would also use the Cellar path, IIUC?

---

_Comment by @ofek on 2024-03-02 03:10_

I don't own a macOS to test that for you unfortunately but that sounds right.

---

_Comment by @charliermarsh on 2024-03-02 03:53_

On `main`, `virtualenv` uses `home = /opt/homebrew/Cellar/python@3.8/3.8.18_2/bin`. So, they might have the same problem @cjolowicz?

---

_Comment by @charliermarsh on 2024-03-02 11:34_

\cc @gaborbernat -- it seems like `virtualenv` on `main` will resolve symlinks for system Pythons, which seems desirable in some cases (hence the issue in `virtualenv`) but not in others (hence this issue). I can't tell which is "correct" though.

---

_Comment by @gaborbernat on 2024-03-03 03:57_

I do not have a good answer here. 😱

---

_Comment by @konstin on 2024-03-03 17:58_

> We may want to revisit this change (https://github.com/astral-sh/uv/issues/965). I think we should only do this (canonicalize) if we're in a virtual environment?

Agreed, i think this is better, i'm also thinking about the use case where somebody might have intentional redirects for their python setup.

For the Cellar issue, note that technically different patch versions aren't compatible from a packaging perspective, technically a project could require `python_full_version != "3.12.2"`. In practice projects i've only seen lower bounds for patch version (e.g. `>3.8.1`), upper or exact bounds would be against how python patch versions are maintained and we're building a more reliable tool by intentionally ignoring this detail. 

---

_Comment by @minusf on 2024-09-13 18:28_

It would be really nice if this was solved somehow, all my uv venvs break after a minor python update...

Besides the difference in the picked executable symlink as described above, I find it interesting that both tools pick the absolute symlink completely differently:

```
pip_venv/bin/python@ -> python3.12
pip_venv/bin/python3@ -> python3.12
pip_venv/bin/python3.12@ -> /opt/homebrew/opt/python@3.12/bin/python3.12

uv_venv/bin/python@ -> /opt/homebrew/Cellar/python@3.12/3.12.6/Frameworks/Python.framework/Versions/3.12/bin/python3.12
uv_venv/bin/python3@ -> python
uv_venv/bin/python3.12@ -> python
```
FWIW my OCD prefers the pip version, the most specific filename has the absolute symlink (`python3.12`) instead of uv's least specific (`python`) 😅 

In case it helps anyone here is also the difference between the `pyvenv.conf`'s:

```diff
--- pip_venv/pyvenv.cfg 2024-09-13 20:18:47.389618788 +0200
+++ uv_venv/pyvenv.cfg  2024-09-13 20:19:00.968279785 +0200
@@ -1,5 +1,6 @@
-home = /opt/homebrew/opt/python@3.12/bin
+home = /opt/homebrew/Cellar/python@3.12/3.12.6/Frameworks/Python.framework/Versions/3.12/bin
+implementation = CPython
+uv = 0.4.9
+version_info = 3.12.6
 include-system-site-packages = false
-version = 3.12.6
-executable = /opt/homebrew/Cellar/python@3.12/3.12.6/Frameworks/Python.framework/Versions/3.12/bin/python3.12
-command = /opt/homebrew/opt/python@3.12/bin/python3.12 -m venv /Volumes/sensitive/src/handmade/0/pip_venv
+relocatable = false
```

---

_Comment by @charliermarsh on 2024-09-23 13:58_

I'm open to changing this since it's been reported multiple times. It might be considered breaking though.

---

_Comment by @frostming on 2024-09-24 08:36_

I suggest changing it to resolve symbolic links until a non-venv python is found.

---

_Comment by @lespea on 2024-09-24 15:23_

I think at the very least it would be nice to have a flag to force one behavior or the other, but maybe there's a concern of bloating commands with too many switches.

---

_Comment by @charliermarsh on 2024-09-24 15:24_

I'm fine to change this but it probably requires 0.5.

---

_Added to milestone `v0.5.0` by @charliermarsh on 2024-09-24 15:24_

---

_Comment by @charliermarsh on 2024-10-21 18:31_

I'll look into this as part of v0.5.

---

_Comment by @charliermarsh on 2024-10-21 19:44_

So just to summarize current behavior:

- `python -m .venv`: Does not resolve symlinks (seemingly, even if you're creating a venv from within a venv -- at least sometimes).
- `virtualenv`: Does fully resolve symlinks (matches uv's behavior). Older versions of virtualenv do _not_ resolve symlinks, but newer versions do, and so suffer from this same problem.
- `uv`: Fully resolves symlinks.

---

_Comment by @charliermarsh on 2024-10-21 19:49_

I don't fully understand the `python -m .venv` behavior. It may have changed ove rtime. For example, if I create a venv with the Homebrew Python, then create a venv from that venv, I get:

```
❯ cat .brew1/pyvenv.cfg
home = /opt/homebrew/opt/python@3.9/bin
include-system-site-packages = false
version = 3.9.19

❯ cat .brew2/pyvenv.cfg
home = /Users/crmarsh/workspace/uv/.brew1/bin
include-system-site-packages = false
version = 3.9.19
```

But if I do the same with my non-Homebrew Python, I get:

```
❯ cat .venv1/pyvenv.cfg
home = /Users/crmarsh/.local/share/rtx/installs/python/3.12.3/bin
include-system-site-packages = false
version = 3.12.3
executable = /Users/crmarsh/.local/share/rtx/installs/python/3.12.3/bin/python3.12
command = /Users/crmarsh/.local/share/rtx/installs/python/3.12.3/bin/python -m venv /Users/crmarsh/workspace/uv/.venv1

❯ cat .venv2/pyvenv.cfg
home = /Users/crmarsh/.local/share/rtx/installs/python/3.12.3/bin
include-system-site-packages = false
version = 3.12.3
executable = /Users/crmarsh/.local/share/rtx/installs/python/3.12.3/bin/python3.12
command = /Users/crmarsh/workspace/uv/.venv1/bin/python -m venv /Users/crmarsh/workspace/uv/.venv2
```

---

_Comment by @charliermarsh on 2024-10-21 20:14_

Ok, if I use Homebrew's 3.12:

```
❯ cat .venv12-1/pyvenv.cfg
home = /opt/homebrew/opt/python@3.12/bin
include-system-site-packages = false
version = 3.12.7
executable = /opt/homebrew/Cellar/python@3.12/3.12.7_1/Frameworks/Python.framework/Versions/3.12/bin/python3.12
command = /opt/homebrew/opt/python@3.12/bin/python3.12 -m venv /Users/crmarsh/workspace/uv/.venv12-1

❯ cat .venv12-2/pyvenv.cfg
home = /opt/homebrew/Cellar/python@3.12/3.12.7_1/Frameworks/Python.framework/Versions/3.12/bin
include-system-site-packages = false
version = 3.12.7
executable = /opt/homebrew/Cellar/python@3.12/3.12.7_1/Frameworks/Python.framework/Versions/3.12/bin/python3.12
command = /Users/crmarsh/workspace/uv/.venv12-1/bin/python -m venv /Users/crmarsh/workspace/uv/.venv12-2
```

So it _fully_ resolves the symlink when inside a virtualenv, and doesn't resolve it when outside a virtualenv.

---

_Comment by @charliermarsh on 2024-10-21 20:14_

What we're suggesting is different than all of these: resolve until we see a non-virtualenv. But I think it makes sense.

---

_Comment by @charliermarsh on 2024-10-21 20:37_

Ok, I'm guessing that the Homebrew Pythons didn't used to set `sys._base_executable`? And now it's set to `/opt/homebrew/Cellar/python@3.12/3.12.7_1/Frameworks/Python.framework/Versions/3.12/bin/python3.12` so they always use that.

---

_Comment by @charliermarsh on 2024-10-21 21:38_

(Gonna put together a table documenting all this.)

---

_Comment by @charliermarsh on 2024-10-21 23:43_

Collated here: https://docs.google.com/spreadsheets/d/1Vw5ClYEjgrBJJhQiwa3cCenIA1GbcRyudYN9NwQaEcM/edit?usp=sharing (sorry, the formatting is terrible)


---

_Comment by @charliermarsh on 2024-10-21 23:44_

uv's current behavior is closest to the new `virtualenv` behavior (which also suffers from the problem described in this issue; prior versions did not). The `venv` behavior is closest to optimal but not quite optimal IMO because it uses `sys._base_executable` which leads to strange outcomes for the nested virtual environment with Brew.

---

_Comment by @charliermarsh on 2024-10-22 00:20_

PR here: https://github.com/astral-sh/uv/pull/8433

---

_Comment by @minusf on 2024-10-22 20:51_

Thank you for looking into this. Out of curiosity, what is the use case of

> then create a venv from that venv

Admittedly I am a very vanilla venv user, always use a homebrew python (or now uv) to create single simple venv's per projects, or until `uv tool install` came along, some handmade `~/usr/venvs/...` for running python apps not installed with brew (esp after 3.12 python started enforcing not to abuse the system wide `site-packages`).  btw `uv tool install` is a real game changer for these and I even uninstalled some brew packages that were lagging a bit behind.

TBH while waiting for this feature to land I was even content just recreating and repopulating the venv's after a python upgrade, it's just so fast from the cache... Although I was a bit surprised that `uv venv` is a no-questions-asked destructive operation, and an existing `.venv` means nothing to it 🤣 

---

_Closed by @zanieb on 2024-11-07 20:29_

---

_Comment by @charliermarsh on 2024-12-10 20:42_

We refined the behavior a bit in https://github.com/astral-sh/uv/pull/9723.

---
