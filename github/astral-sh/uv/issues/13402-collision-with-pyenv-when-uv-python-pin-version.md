---
number: 13402
title: "Collision with pyenv when `uv python pin <version>`"
type: issue
state: closed
author: hoxell
labels:
  - bug
assignees: []
created_at: 2025-05-12T09:47:51Z
updated_at: 2025-06-18T17:56:55Z
url: https://github.com/astral-sh/uv/issues/13402
synced_at: 2026-01-07T13:12:18-06:00
---

# Collision with pyenv when `uv python pin <version>`

---

_Issue opened by @hoxell on 2025-05-12 09:47_

### Summary

### Issue
On my work-managed machine, I have `pyenv` installed (i.e., non-optional), which seems to be conflicting with `uv`. When the current pinned version is set to a version that's not installed by `pyenv` (here, 3.13), I get:

```
error: Failed to inspect Python interpreter from first executable in the search path at `~/.pyenv/shims/python3.10`
  Caused by: Querying Python at `~/.pyenv/shims/python3.10` failed with exit status exit status: 1

[stderr]
pyenv: version `3.13' is not installed (set by ~/some_project/.python-version)
```

If I manually set it to `3.12` in `.python-version` instead, which is installed via `pyenv` already, there's a slightly different error:

```
error: Failed to inspect Python interpreter from first executable in the search path at `~/.pyenv/shims/python3.10`
  Caused by: Querying Python at `~/.pyenv/shims/python3.10` failed with exit status exit status: 127

[stderr]
pyenv: python3.10: command not found

The `python3.10' command exists in these Python versions:
  3.10.16

Note: See 'pyenv help global' for tips on allowing both
      python2 and python3 to be found.
```

### Reproducing
`cd <some_sub_of_project>`
`uv python pin 3.13`
`uv python pin 3.10`

That is, both tools using `.python-version` seems to break the discovery since `pyenv` picks up `.python-version` in a subdirectory of the project.

As long as the `pwd` is the project root, it works fine.

Of course, it's `pyenv` struggling,  but might be a situation that `uv` would like to handle.

Btw, I only discovered this because I accidentally was in a subdir when trying to pin the version. Maybe there's a use case for multiple `.python-version` in a single project, but for my use case(s), it would've been more natural if it would set the version in the project root.

And lastly, a big thank you for such an amazing tool 😍 

### Platform

Ubuntu 24.04 x86_64

### Version

0.7.3

### Python version

_No response_

---

_Label `bug` added by @hoxell on 2025-05-12 09:47_

---

_Comment by @konstin on 2025-05-12 09:55_

Do you have a reproducer how to setup pyenv in a way that causes `~/.pyenv/shims/python3.10` to exist but error that `3.13` is not installed?

---

_Label `needs-mre` added by @konstin on 2025-05-12 09:55_

---

_Comment by @hoxell on 2025-05-13 06:39_

I can reproduce this by:
1. `pyenv uninstall 3.10.16` (or whatever version you have installed. I only did this to make sure I had a clean install, it didn't seem to make a difference.)
2. `pyenv install 3.10.16`
3. From the subdir of the uv project, `uv python pin <some version>`. The version can be either a version you have installed via `pyenv` or a version you don't have installed. Either of them should trigger one of the errors in the original post.
4. `uv python pin <some version available via pyenv>`

I noticed some inconsistent behaviour in reproducing this. Sometimes starting a simple repl with `python<major>.<minor>` would work, sometimes it wouldn't, failing with some variant of:

```
pyenv: python3.10: command not found

The `python3.10' command exists in these Python versions:
  3.10.16

Note: See 'pyenv help global' for tips on allowing both
      python2 and python3 to be found.
```

`PATH` also is set correctly, so all versions I tried were resolved via `~/.pyenv/shims`.

It's old, but I don't see any changes mentioned regarding the design, which seems to mean `pyenv` does _not_ intend to support _not_ specifying also the patch version explicitly:
https://github.com/pyenv/pyenv/issues/34#issuecomment-21294584

Possibly, the inconsistent behaviour above could mean this design might not be fully enforced.

---

_Comment by @konstin on 2025-05-13 07:44_

uv is already ignoring the error by default, but it will resurface the error if no other appropriate interpreter is is found. If not deactivated, uv installs a managed Python interpreter. By deactivating managed interpreters, uv can't resolve itself out of pyenv's missing version as it usually would. It seems correct to me to surface the error so the user can adjust their pyenv config.

```
$ rm -rf ../.venv && uv run -p python3.13 --no-managed-python --no-cache -v python
DEBUG uv 0.7.3
DEBUG Found project root: `/home/konsti/projects/pyenv-version-detection`
DEBUG No workspace root found, using project root
DEBUG Discovered project `pyenv-version-detection` at: /home/konsti/projects/pyenv-version-detection
DEBUG Acquired lock for `/home/konsti/projects/pyenv-version-detection`
DEBUG Using Python request `3.13` from explicit request
DEBUG Checking for Python environment at `/home/konsti/projects/pyenv-version-detection/.venv`
DEBUG Searching for Python 3.13 in search path
DEBUG Failed to inspect Python interpreter from first executable in the search path at `/home/konsti/.pyenv/shims/python3.13` 
DEBUG Skipping bad interpreter at /home/konsti/.pyenv/shims/python3.13 from first executable in the search path: Querying Python at `/home/konsti/.pyenv/shims/python3.13` failed with exit status exit status: 1

[stderr]
pyenv: version `3.10.1234' is not installed (set by /home/konsti/projects/pyenv-version-detection/subdir/.python-version)

DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `/home/konsti/.pyenv/shims/python3` (search path)
DEBUG Skipping interpreter at `/usr/bin/python3` from search path: does not satisfy request `3.13`
DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `/home/konsti/.pyenv/shims/python` (search path)
DEBUG Skipping interpreter at `/usr/bin/python` from search path: does not satisfy request `3.13`
DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `/usr/bin/python3` (search path)
DEBUG Skipping interpreter at `/usr/bin/python3` from search path: does not satisfy request `3.13`
DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `/usr/bin/python` (search path)
DEBUG Skipping interpreter at `/usr/bin/python` from search path: does not satisfy request `3.13`
DEBUG Released lock at `/tmp/uv-303ffab34f2a5b53.lock`
error: Failed to inspect Python interpreter from first executable in the search path at `/home/konsti/.pyenv/shims/python3.13`
  Caused by: Querying Python at `/home/konsti/.pyenv/shims/python3.13` failed with exit status exit status: 1

[stderr]
pyenv: version `3.10.1234' is not installed (set by /home/konsti/projects/pyenv-version-detection/subdir/.python-version)
```

---

_Label `bug` removed by @konstin on 2025-05-13 07:44_

---

_Label `needs-mre` removed by @konstin on 2025-05-13 07:44_

---

_Label `question` added by @konstin on 2025-05-13 07:44_

---

_Comment by @hoxell on 2025-05-13 08:10_

That sounds like the reasonable thing to do, but I don't think I've deactivated managed python versions.
The `pyproject.toml` contains no such configuration, and neither does `~/.config/uv/uv.toml`. I have no `/etc/uv` config dir, so that's not interfering, and there's no `uv.toml` file in the project.
Those are the locations I could find [here](https://docs.astral.sh/uv/configuration/files/), but maybe I've overlooked something?

I don't have python 3.11 installed anywhere, but pinning it to 3.11 (from the project root, not a subdir) works as expected, and on `uv run ...` it'll pull a managed version.

---

_Comment by @hoxell on 2025-05-13 08:21_

I just encountered the same issue in the project root now, so it's not isolated to subdirs (for me) as it initially seemed.

---

_Comment by @konstin on 2025-05-13 08:53_

Can you share the log from running with `-v`?

---

_Comment by @hoxell on 2025-05-13 11:49_

My bad, sorry. Totally slipped my mind to include the verbose output 🤦‍♂️

Created a fresh project, `uv init --package pin`, and ran `uv python pin 3.13` before trying to pin it to 3.10 below.

```
~/pin (main ✗)  uv python pin 3.10 -v
DEBUG uv 0.7.3
DEBUG Found project root: `/home/xxxxxxx/pin`
DEBUG No workspace root found, using project root
DEBUG Reading Python requests from version file at `/home/xxxxxxx/pin/.python-version`
DEBUG Searching for Python 3.10 in managed installations or search path
DEBUG Searching for managed installations at `/home/xxxxxxx/.local/share/uv/python`
DEBUG Skipping incompatible managed installation `cpython-3.13.2-linux-x86_64-gnu`
DEBUG Skipping incompatible managed installation `cpython-3.11.12-linux-x86_64-gnu`
DEBUG Failed to inspect Python interpreter from first executable in the search path at `/home/xxxxxxx/.pyenv/shims/python3.10`
DEBUG Skipping bad interpreter at /home/xxxxxxx/.pyenv/shims/python3.10 from first executable in the search path: Querying Python at `/home/xxxxxxx/.pyenv/shims/python3.10` failed with exit status exit status: 1

[stderr]
pyenv: version `3.13' is not installed (set by /home/xxxxxxx/pin/.python-version)

DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `/home/xxxxxxx/.pyenv/shims/python3` (search path)
DEBUG Skipping interpreter at `/usr/bin/python3` from search path: does not satisfy request `3.10`
DEBUG Failed to inspect Python interpreter from search path at `/home/xxxxxxx/.pyenv/shims/python`
DEBUG Skipping bad interpreter at /home/xxxxxxx/.pyenv/shims/python from search path: Querying Python at `/home/xxxxxxx/.pyenv/shims/python` failed with exit status exit status: 1

[stderr]
pyenv: version `3.13' is not installed (set by /home/xxxxxxx/pin/.python-version)

DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `/usr/bin/python3` (search path)
DEBUG Skipping interpreter at `/usr/bin/python3` from search path: does not satisfy request `3.10`
error: Failed to inspect Python interpreter from first executable in the search path at `/home/xxxxxxx/.pyenv/shims/python3.10`
  Caused by: Querying Python at `/home/xxxxxxx/.pyenv/shims/python3.10` failed with exit status exit status: 1

[stderr]
pyenv: version `3.13' is not installed (set by /home/xxxxxxx/pin/.python-version)
```

```
~  pyenv versions
  system
  3.8.20
* 3.10.14 (set by /home/xxxxxxx/.pyenv/version)
  3.12.9
```

```
~ echo $PATH
/home/xxxxxxx/.pyenv/shims:/home/xxxxxxx/.pyenv/bin:<and more stuff not related to pyenv>
```

---

_Renamed from "Collision with pyenv when `uv python pin <version>` from subdir" to "Collision with pyenv when `uv python pin <version>`" by @hoxell on 2025-05-13 12:29_

---

_Comment by @konstin on 2025-05-13 13:02_

I'm still having trouble reproducing this, can you add `--managed-python` and see what happens?

---

_Comment by @hoxell on 2025-05-13 13:55_

Certainly, and that worked:

```
~/pin (main ✗)   uv python pin 3.10 --managed-python -v
DEBUG uv 0.7.3
DEBUG Found project root: `/home/xxxxxxx/pin`
DEBUG No workspace root found, using project root
DEBUG Reading Python requests from version file at `/home/xxxxxxx/pin/.python-version`
DEBUG Searching for Python 3.10 in managed installations
DEBUG Searching for managed installations at `/home/xxxxxxx/.local/share/uv/python`
DEBUG Skipping incompatible managed installation `cpython-3.13.2-linux-x86_64-gnu`
DEBUG Skipping incompatible managed installation `cpython-3.11.12-linux-x86_64-gnu`
warning: No interpreter found for Python 3.10 in managed installations
DEBUG Discovered project `pin` at: /home/xxxxxxx/pin
DEBUG Writing Python versions to `/home/xxxxxxx/pin/.python-version`
Updated `.python-version` from `3.13` -> `3.10`
```

When trying to reproduce it, did you only `pyenv install` one version? If so, could you try installing three or more versions?
I noticed that `pyenv` would work with two out of three versions when I only specified the major and minor version, so maybe if you install three different (minor) versions and try pinning all of them you might be able to reproduce this? 🤞

---

_Label `needs-mre` added by @konstin on 2025-05-15 08:23_

---

_Comment by @konstin on 2025-05-15 08:23_

Still can't reproduce this unfortunately:

```
$ rm .python-version
$ pyenv versions
  system
  3.7.17
  3.8.12
  3.8.18
  3.9.18
  3.10.14
* 3.10.16 (set by /home/konsti/.pyenv/version)
* 3.11.7 (set by /home/konsti/.pyenv/version)
* 3.12.3 (set by /home/konsti/.pyenv/version)
  pypy3.10-7.3.14
$ python3.13
Command 'python3.13' not found, did you mean:
  command 'python3.11' from deb python3.11 (3.11.7-2)
  command 'python3.12' from deb python3.12 (3.12.3-1ubuntu0.5)
Try: sudo apt install <deb name>
$ uv python pin 3.13 -v
DEBUG uv 0.7.3
DEBUG Found project root: `/home/konsti/projects/pyenv-version-detection`
DEBUG No workspace root found, using project root
DEBUG No Python version file found in ancestors of working directory: /home/konsti/projects/pyenv-version-detection
DEBUG Searching for Python 3.13 in managed installations or search path
DEBUG Searching for managed installations at `/home/konsti/.local/share/uv/python`
DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `/usr/bin/python3` (first executable in the search path)
DEBUG Skipping interpreter at `/usr/bin/python3` from first executable in the search path: does not satisfy request `3.13`
DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `/usr/bin/python` (search path)
DEBUG Skipping interpreter at `/usr/bin/python` from search path: does not satisfy request `3.13`
warning: No interpreter found for Python 3.13 in managed installations or search path
DEBUG Discovered project `pyenv-version-detection` at: /home/konsti/projects/pyenv-version-detection
DEBUG Writing Python versions to `/home/konsti/projects/pyenv-version-detection/.python-version`
Pinned `.python-version` to `3.13`
```

Can you try providing a Dockerfile that reproduces the problem? That is the most reliable way to provide an MRE.

---

_Comment by @hoxell on 2025-05-15 11:22_

Managed to reproduce it with this `Dockerfile`:

```
FROM ubuntu:24.04

RUN apt update && \
    apt install -y curl python3 python3-pip git build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev && \
    apt clean && \
    rm -rf /var/lib/apt/lists/*

RUN curl -fsSL https://pyenv.run | bash && \
    echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc && \
    echo '[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc && \
    echo 'eval "$(pyenv init - bash)"' >> ~/.bashrc && \
    bash -c -i "source ~/.bashrc && pyenv install 3.8.20 3.10.16 3.12.9" && \
    curl -LsSf https://astral.sh/uv/0.7.3/install.sh | sh &&\
    echo "3.10.16" > ~/.pyenv/version
```

```
~ docker run --rm -it my_uv
root@92607cd4167e:/# uv init --package pin
Initialized project `pin` at `/pin`
root@92607cd4167e:/# cd pin
root@92607cd4167e:/pin# uv python pin 3.13
warning: No interpreter found for Python 3.13 in managed installations or search path
Updated `.python-version` from `3.10` -> `3.13`
root@92607cd4167e:/pin# uv python pin 3.10
error: Failed to inspect Python interpreter from first executable in the search path at `/root/.pyenv/shims/python3.10`
  Caused by: Querying Python at `/root/.pyenv/shims/python3.10` failed with exit status exit status: 1

[stderr]
pyenv: version `3.13' is not installed (set by /pin/.python-version)
```

edit: If you supply `--managed-python`, it works, just like it did above.

edit2: The verbose output of the above `pin`:

```
root@92607cd4167e:/pin# uv python pin 3.10 -v
DEBUG uv 0.7.3
DEBUG Found project root: `/pin`
DEBUG No workspace root found, using project root
DEBUG Reading Python requests from version file at `/pin/.python-version`
DEBUG Searching for Python 3.10 in managed installations or search path
DEBUG Searching for managed installations at `/root/.local/share/uv/python`
DEBUG Failed to inspect Python interpreter from first executable in the search path at `/root/.pyenv/shims/python3.10`
DEBUG Skipping bad interpreter at /root/.pyenv/shims/python3.10 from first executable in the search path: Querying Python at `/root/.pyenv/shims/python3.10` failed with exit status exit status: 1

[stderr]
pyenv: version `3.13' is not installed (set by /pin/.python-version)

DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `/root/.pyenv/shims/python3` (search path)
DEBUG Skipping interpreter at `/usr/bin/python3` from search path: does not satisfy request `3.10`
DEBUG Failed to inspect Python interpreter from search path at `/root/.pyenv/shims/python`
DEBUG Skipping bad interpreter at /root/.pyenv/shims/python from search path: Querying Python at `/root/.pyenv/shims/python` failed with exit status exit status: 1

[stderr]
pyenv: version `3.13' is not installed (set by /pin/.python-version)

DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `/usr/bin/python3` (search path)
DEBUG Skipping interpreter at `/usr/bin/python3` from search path: does not satisfy request `3.10`
error: Failed to inspect Python interpreter from first executable in the search path at `/root/.pyenv/shims/python3.10`
  Caused by: Querying Python at `/root/.pyenv/shims/python3.10` failed with exit status exit status: 1

[stderr]
pyenv: version `3.13' is not installed (set by /pin/.python-version)

```

---

_Label `needs-mre` removed by @konstin on 2025-05-16 13:07_

---

_Label `question` removed by @konstin on 2025-05-16 16:02_

---

_Label `bug` added by @konstin on 2025-05-16 16:02_

---

_Comment by @konstin on 2025-05-16 20:51_

CC @zanieb do you know what is causing this?

---

_Comment by @hoxell on 2025-05-17 10:14_

I think I've found the reason.
https://github.com/astral-sh/uv/blob/9d1a14e1f9d824d8b4932292cbcf304f51c2ada9/crates/uv-python/src/discovery.rs#L1131

```
    // If we found a Python, but it was unusable for some reason, report that instead of saying we
    // couldn't find any Python interpreters.
    if let Some(err) = first_error {
        return Err(err);
    }
```

When trying to pin 3.10, `uv` detects a set of candidate interpreters, but they're either the wrong version or unusable. Thus, `first_error` will be `Some(...)` and an `Err` will be returned.

When adding `--managed-python`, the discovery will ignore any system interpreters, `first_error` will remain `None`, and an `Ok(Err(PythonNotFound))` will be returned instead, which will not bubble up as an `Result<ExistStatus>` error from https://github.com/astral-sh/uv/blob/9d1a14e1f9d824d8b4932292cbcf304f51c2ada9/crates/uv/src/commands/python/pin.rs#L95

The original issue with a collision of `.python-version` files between `uv` and `pyenv` is unfortunate, but I can actually see why this design choice was made, not to silently ignore a broken python installation for `PythonPreference::Managed`.
After all, this preference means managed python installations should be preferred over system installations, but system installations over downloading a managed version.
I suppose it comes down to predictability vs. convenience.

I'm not sure how it would be received if it triggered a warning instead of a hard failure, but I think I can survive adding `--managed-python` whenever I need to change the pinned version.

---

_Assigned to @zanieb by @zanieb on 2025-05-18 20:46_

---

_Comment by @hoxell on 2025-06-18 17:56_

I can't repro this anymore (0.7.13). Thanks for fixing 🙏 

How I tried to repro:
```
FROM ubuntu:24.04

RUN apt update && \
    apt install -y curl python3 python3-pip git build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev && \
    apt clean && \
    rm -rf /var/lib/apt/lists/*

RUN curl -fsSL https://pyenv.run | bash && \
    echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc && \
    echo '[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc && \
    echo 'eval "$(pyenv init - bash)"' >> ~/.bashrc && \
    bash -c -i "source ~/.bashrc && pyenv install 3.8.20 3.10.16 3.12.9" && \
    echo "3.10.16" > ~/.pyenv/version

ENV PATH="/root/.local/bin:$PATH"
#Changing to the original (commented) line still reproduces the issue
#RUN curl -LsSf https://astral.sh/uv/0.7.3/install.sh | sh && \
RUN curl -LsSf https://astral.sh/uv/0.7.13/install.sh | sh && \
    uv init --package --python 3.10 pin && \
    cd pin && \
    uv python pin 3.13 && \
    bash -c -i "source ~/.bashrc && uv python pin 3.10"
```

---

_Closed by @hoxell on 2025-06-18 17:56_

---
