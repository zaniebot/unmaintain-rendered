```yaml
number: 8481
title: Use base executable to set virtualenv Python path
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - breaking
assignees: []
merged: true
base: tracking/050
head: charlie/base-exec
created_at: 2024-10-22T23:16:52Z
updated_at: 2024-12-10T20:42:15Z
url: https://github.com/astral-sh/uv/pull/8481
synced_at: 2026-01-10T11:59:59Z
```

# Use base executable to set virtualenv Python path

---

_Pull request opened by @charliermarsh on 2024-10-22 23:16_

## Summary

See extensive discussion in https://github.com/astral-sh/uv/pull/8433#issuecomment-2430472849.

This PR brings us into alignment with the standard library by using `sys._base_executable` rather than canonicalizing the executable path.

The benefits are primarily for Homebrew, where we'll now resolve to paths like `/opt/homebrew/opt/python@3.12/bin` instead of the undesirable `/opt/homebrew/Cellar/python@3.9/3.9.19_1/Frameworks/Python.framework/Versions/3.9/bin`.

Most other users should see no change, though in some cases, nested virtual environments now have slightly different behavior -- namely, they _sometimes_ resolve to the virtual environment Python (at least for Homebrew; not for rtx or uv Pythons though). See [here](https://docs.google.com/spreadsheets/d/1Vw5ClYEjgrBJJhQiwa3cCenIA1GbcRyudYN9NwQaEcM/edit?gid=0#gid=0) for a breakdown.

Closes https://github.com/astral-sh/uv/issues/1640.
Closes https://github.com/astral-sh/uv/issues/1795.


---

_Review requested from @zanieb by @charliermarsh on 2024-10-22 23:17_

---

_Review requested from @konstin by @charliermarsh on 2024-10-22 23:17_

---

_Label `enhancement` added by @charliermarsh on 2024-10-22 23:18_

---

_Label `breaking` added by @charliermarsh on 2024-10-22 23:18_

---

_Comment by @charliermarsh on 2024-10-22 23:22_

Oh, I did find one Python that does not define `sys._base_executable`: `/Library/Frameworks/Python.framework/Versions/3.7/bin/python3`

---

_@charliermarsh reviewed on 2024-10-22 23:24_

---

_Review comment by @charliermarsh on `crates/uv-virtualenv/src/virtualenv.rs`:63 on 2024-10-22 23:24_

CPython errors if `sys._base_executable` is undefined, at least on latest. But we have to support creating environments from _other_ Pythons. I found that in 3.7 at least, they used this logic.

---

_Comment by @charliermarsh on 2024-10-22 23:34_

Okay, it looks like we have the ~same problem with our test setup: we create symlinks to Pythons, but they lose the connection to the base interpreter.

---

_Comment by @ofek on 2024-10-22 23:45_

What do you think about flag(s) to modify behavior to account for situations that are broken by the default implementation?

---

_Comment by @charliermarsh on 2024-10-22 23:46_

What is an example of a real situation that is broken by this implementation?

---

_Review comment by @charliermarsh on `crates/uv-virtualenv/src/virtualenv.rs`:85 on 2024-10-22 23:54_

This is my attempt to solve the problem demonstrated by our test suite. We symlink from the installed Python directory to (e.g.) `/Users/crmarsh/.local/share/uv/tests/.tmpIl4Pjf/python/3.12/python3`. In `python-build-standalone`, `sys._base_executable` for that symlink just points to the current Python (i.e., it points to `/Users/crmarsh/.local/share/uv/tests/.tmpIl4Pjf/python/3.12/python3`). The trick I'm employing here is that we resolve symlinks as long as the binary is not in the `scripts` path.

---

_@charliermarsh reviewed on 2024-10-22 23:54_

---

_@konstin reviewed on 2024-10-23 10:06_

---

_Review comment by @konstin on `crates/uv-virtualenv/src/virtualenv.rs`:63 on 2024-10-23 10:06_

i checked and it's also set in pypy3.10 on linux

---

_@konstin reviewed on 2024-10-23 10:14_

---

_Review comment by @konstin on `crates/uv-virtualenv/src/virtualenv.rs`:85 on 2024-10-23 10:14_

Does this only occur with python build standalone? In this case, we should patch this in pbs

---

_@charliermarsh reviewed on 2024-10-23 11:32_

---

_Review comment by @charliermarsh on `crates/uv-virtualenv/src/virtualenv.rs`:85 on 2024-10-23 11:32_

Patch it to what?

---

_@konstin reviewed on 2024-10-23 13:20_

---

_Review comment by @konstin on `crates/uv-virtualenv/src/virtualenv.rs`:85 on 2024-10-23 13:20_

i think we have to fix https://github.com/indygreg/python-build-standalone/issues/380, or at least i expect it's closely related.

---

_@charliermarsh reviewed on 2024-10-24 02:17_

---

_Review comment by @charliermarsh on `crates/uv-virtualenv/src/virtualenv.rs`:85 on 2024-10-24 02:17_

What do you think of the solution itself? Alternatively, we could determine whether this is a uv-managed Python, and resolve the symlink if so. (But we might then fail if users install PBS builds through some other mechanism and symlink _those_.)

---

_Comment by @konstin on 2024-10-24 09:51_

I can't test the homebrew situation, though unlike #8433, it doesn't solve my pyenv use case:

I have a `/home/konsti/.local/bin/python3.11` that is symlinked to a pyenv 3.11 in `/home/konsti/.pyenv/versions/3.11.7/bin` (pyenv tags folders with the patch version, you can install multiple patch releases at the same time). It would be nice if I could change the symlink when a new patch version of 3.11 is released, without breaking all existing venvs. With this branch, we resolve through the symlink:

```console
$ uv-branch venv -p /home/konsti/.local/bin/python3.11 branch
$ uv-main venv -p /home/konsti/.local/bin/python3.11 main
$ ls -lah main/bin/python
lrwxrwxrwx 1 konsti konsti 50 Okt 24 11:45 main/bin/python -> /home/konsti/.pyenv/versions/3.11.7/bin/python3.11
$ ls -lah branch/bin/python
lrwxrwxrwx 1 konsti konsti 46 Okt 24 11:45 branch/bin/python -> /home/konsti/.pyenv/versions/3.11.7/bin/python
```

This is not a blocker if it solves the homebrew situation more similar to std than #8433, I just want to note this drawback here.

---

_@konstin reviewed on 2024-10-24 09:52_

---

_Review comment by @konstin on `crates/uv-virtualenv/src/virtualenv.rs`:85 on 2024-10-24 09:52_

(replied in the main thread)

---

_@konstin approved on 2024-10-24 09:52_

---

_Comment by @zanieb on 2024-10-24 12:48_

Similarly, this does not solve the version stability goals for our managed Python installations as mentioned in https://github.com/astral-sh/uv/pull/8433#issuecomment-2428037536 and as implemented in #8458. As @konstin concludes, I think this is okay since we're aligning with CPython, but it'd be nice to have some plan to address this. It's possible the solution is "create something that looks like Homebrew instead", where you have a whole lib directory structure for minor Python versions. I'm not excited about the additional indirection, but it does seem roughly more correct. We may be able to encourage pyenv to move in the same direction? Perhaps we should see if they have something to say about this?

---

_Comment by @zanieb on 2024-10-24 12:57_

I guess an open question for me is if we want to provide a setting for this, i.e., `UV_VENV_PYTHON_LINK = resolve | base | no-resolve` (don't focus on the names here), so people can recover the existing behavior if we break their setup (and pyenv users can get the behavior they need immediately)

---

_Comment by @charliermarsh on 2024-10-24 13:00_

Critically, though, it would be actively wrong to use `/home/konsti/.local/bin` as the virtual environment `home`. The same is true for the managed Python design, right? That's not a valid Python installation. It might work, but it also might not, and it could do things we don't expect or can't test (I just don't know how `home` is used precisely). Homebrew is actually doing something fairly different whereby it's setting `sys._base_executable` to a stable symlink to a valid Python installation. I think that problem needs to be solved in the Python distributions and layouts, and not in uv...

(I know you both know some of this, just repeating it for clarity.)

---

_Comment by @zanieb on 2024-10-24 13:45_

> Critically, though, it would be actively wrong to use /home/konsti/.local/bin as the virtual environment home. The same is true for the managed Python design, right? That's not a valid Python installation.

Totally agree. I wonder if we can just omit `home` entirely in that case? I mean, since it's not valid it basically has no affect and falls back to information present in the Python installation. I tried removing it from a Homebrew virtual environment's `pyvenv.cfg` and nothing broke.

Switching to a "base executable" strategy by default (and presuming `home` is always correct) but _not_ writing `home` for an alternative strategy where we don't follow links might be reasonable? Of course, this doesn't work for our managed distributions (probably because of #8429) but I think that's a separate problem we can address.

---

_Comment by @charliermarsh on 2024-10-24 13:46_

Yeah agreed. I think we need to learn more about how `home` works and the intended design (and see if we can fix the managed distribution behavior upstream). At least this change here brings us into alignment with the standard library by default though, rather than further from it, which seems good.

---

_Comment by @zanieb on 2024-10-24 14:03_

Yeah all for changing the default. I'm also leaning further towards the idea that we should support configuring the strategy as an escape hatch for workflows this change breaks though; even if we only support this new mode (`base-executable`) and our existing behavior (`resolve`) to start.

---

_Comment by @charliermarsh on 2024-10-24 15:15_

I asked in https://github.com/indygreg/python-build-standalone/issues/382 if there's a way we can reliably detect relocatable Pythons.

---

_Comment by @charliermarsh on 2024-10-25 01:13_

Ok, to workaround the python-build-standalone bug, I'm now doing:

1. For python-build-standalone (using the heuristic from the linked issue), resolve all symlinks.
2. Follow CPython for all other interpreters.

The only risk here is that the heuristic could have false positives (interpreters with a legitimate prefix of `/install`), but I guess we just accept that. Even in that case, the strategy is still ~fine -- you just get the behavior we have today on `main`.


---

_Review requested from @konstin by @charliermarsh on 2024-10-25 01:13_

---

_Comment by @charliermarsh on 2024-10-25 01:33_

(I'm sort of hesitant to add settings for the strategy without clear demand.)

---

_Comment by @zanieb on 2024-10-25 04:09_

> (I'm sort of hesitant to add settings for the strategy without clear demand.)

Fair, but we already have some clear cases where we need to resolve the link to avoid breaking everything (e.g., for our own Python distributions). I think making this breaking change without a way to recover the previous behavior is pretty risky. I think just an environment variable is fine, not looking for first-class configuration options.

---

_@zanieb approved on 2024-10-25 04:16_

---

_Comment by @konstin on 2024-10-25 08:14_

Here's how `sys._base_executable` and `pyvenv.cfg`'s home look on my ubuntu 24.04 for a system interpreter, a pyenv interpreter and a python build standalone.

### Basic usage, no symlinks

System interpreter
```
$ /usr/bin/python3 -c "import sys; print(sys._base_executable)"
/usr/bin/python3
$ rm -rf venv && /usr/bin/python3 -m venv venv && cat venv/pyvenv.cfg | grep "home "
home = /usr/bin
```

Pyenv interpreter
```
$ /home/konsti/.pyenv/versions/3.11.7/bin/python3.11 -c "import sys; print(sys._base_executable)"
/home/konsti/.pyenv/versions/3.11.7/bin/python3.11
$ rm -rf venv && /home/konsti/.pyenv/versions/3.11.7/bin/python3.11 -m venv venv && cat venv/pyvenv.cfg | grep "home "
home = /home/konsti/.pyenv/versions/3.11.7/bin
```

Python build standalone
```
$ /home/konsti/.local/share/uv/python/cpython-3.11.10-linux-x86_64-gnu/bin/python3 -c "import sys; print(sys._base_executable)"
/home/konsti/.local/share/uv/python/cpython-3.11.10-linux-x86_64-gnu/bin/python3
$ rm -rf venv && /home/konsti/.local/share/uv/python/cpython-3.11.10-linux-x86_64-gnu/bin/python3 -m venv venv && cat venv/pyvenv.cfg | grep "home "
home = /home/konsti/.local/share/uv/python/cpython-3.11.10-linux-x86_64-gnu/bin
```

### Symlinked interpreters

```
ln -s /usr/bin/python3 python-system
ln -s /home/konsti/.pyenv/versions/3.11.7/bin/python3.11 python-pyenv
ln -s /home/konsti/.local/share/uv/python/cpython-3.11.10-linux-x86_64-gnu/bin/python3 python-pbs
```

```
$ ./python-system -c "import sys; print(sys._base_executable)"
/home/konsti/projects/a/python-system
$ ./python-pyenv -c "import sys; print(sys._base_executable)"
/home/konsti/projects/a/python-pyenv
$ ./python-pbs -c "import sys; print(sys._base_executable)"
/home/konsti/projects/a/python-pbs
```

### venvs from symlinked interpreters

```
$ rm -rf venv && ./python-system -m venv --without-pip venv && cat venv/pyvenv.cfg | grep "home "
home = /home/konsti/projects/a
$ venv/bin/python -c "import sys; print(sys._base_executable)"
/usr/bin/python3.12
$ rm -rf venv && ./python-pyenv -m venv --without-pip venv && cat venv/pyvenv.cfg | grep "home "
home = /home/konsti/projects/a
$ venv/bin/python -c "import sys; print(sys._base_executable)"
/home/konsti/.pyenv/versions/3.11.7/bin/python3.11
$ rm -rf venv && ./python-pbs -m venv --without-pip venv && cat venv/pyvenv.cfg | grep "home "
home = /home/konsti/projects/a
$ venv/bin/python -c "import sys; print(sys._base_executable)"
[ERROR: ModuleNotFoundError: No module named 'encodings']
```


---

_@konstin approved on 2024-10-25 08:17_

---

_Comment by @charliermarsh on 2024-10-25 13:08_

I'm just concerned that users will use such a setting without understanding all the nuances and break their own setups or do things that are slightly and silently wrong. The tradeoffs are really complicated, and any bugs that emerge as a result of this change are in line with the standard library and bugs in those Python distributions. I understand the push though... I can add it as an environment variable, as an escape hatch.

---

_Merged by @charliermarsh on 2024-10-28 16:06_

---

_Closed by @charliermarsh on 2024-10-28 16:06_

---

_Branch deleted on 2024-10-28 16:06_

---

_Comment by @charliermarsh on 2024-12-10 20:42_

We refined the behavior a bit in https://github.com/astral-sh/uv/pull/9723.

---
