---
number: 15923
title: uv pip install unexpectedly works against unsourced environment in ~/.local/
type: issue
state: open
author: dvrogozh
labels:
  - question
assignees: []
created_at: 2025-09-18T00:36:51Z
updated_at: 2025-09-18T12:57:00Z
url: https://github.com/astral-sh/uv/issues/15923
synced_at: 2026-01-10T01:26:01Z
---

# uv pip install unexpectedly works against unsourced environment in ~/.local/

---

_Issue opened by @dvrogozh on 2025-09-18 00:36_

### Summary

I was experimenting with installation options which `uv` provides and came across the following scenario which seems quote confusing. It occurs that if `uv` virtual environment is created in `~/.local/` folder, then some of the `uv pip` commands, specifically - `uv pip install` - start to pick it up even if environment is not activated while other commands, such as `uv pip show` and `uv pip list` ignore it.

Can either the whole this behavior be prohibited entirely (`uv pip install` to be failed) or allowed and then all the `uv` commands should support such scenario?

```
docker run -it --rm ubuntu:24.04
root@b7d54f53e6ef:/# apt-get update && apt-get install curl
root@b7d54f53e6ef:/# curl -LsSf https://astral.sh/uv/install.sh | sh
downloading uv 0.8.18 x86_64-unknown-linux-gnu
no checksums to verify
installing to /root/.local/bin
  uv
  uvx
everything's installed!

To add $HOME/.local/bin to your PATH, either restart your shell or run:

    source $HOME/.local/bin/env (sh, bash, zsh)
    source $HOME/.local/bin/env.fish (fish)
root@b7d54f53e6ef:/# source $HOME/.local/bin/env

root@b7d54f53e6ef:/# uv pip install numpy
error: No virtual environment found; run `uv venv` to create an environment, or pass `--system` to install into a non-virtual environment

root@b7d54f53e6ef:/# uv venv --allow-existing $HOME/.local
Using CPython 3.13.7
Creating virtual environment at: /root/.local
Activate with: source root/.local/bin/activate

root@b7d54f53e6ef:/# uv pip install numpy
Using Python 3.13.7 environment at: /root/.local
Resolved 1 package in 111ms
Prepared 1 package in 385ms
Installed 1 package in 61ms
 + numpy==2.3.3

root@b7d54f53e6ef:/# uv pip show numpy
Using Python 3.13.7 environment at: /root/.local/share/uv/python/cpython-3.13.7-linux-x86_64-gnu
warning: Package(s) not found for: numpy

root@b7d54f53e6ef:/# uv pip list
Using Python 3.13.7 environment at: /root/.local/share/uv/python/cpython-3.13.7-linux-x86_64-gnu
Package Version
------- -------
pip     24.3.1
```

### Platform

Ubuntu 24.04

### Version

0.8.18

### Python version

Python 3.13.7

---

_Label `bug` added by @dvrogozh on 2025-09-18 00:36_

---

_Comment by @zanieb on 2025-09-18 00:52_

That's weird, thanks for the reproduction — I'll take a look.

---

_Comment by @zanieb on 2025-09-18 00:55_

Oh, it's because you're creating a virtual environment at `~/.local` which puts a `python` binary in `~/.local/bin` which we then find since `~/.local/bin` is on your `PATH`

```
TRACE Searching PATH for executables: python, python3
TRACE Checking `PATH` directory for interpreters: /root/.local/bin
TRACE Found possible Python executable: /root/.local/bin/python
TRACE Found cached interpreter info for Python 3.13.7, skipping query of: /root/.local/bin/python
DEBUG Found `cpython-3.13.7-linux-aarch64-gnu` at `/root/.local/bin/python` (first executable in the search path)
```

I'd recommend not doing that :)

Perhaps you meant `uv venv ~/.local/venv`?

---

_Label `bug` removed by @zanieb on 2025-09-18 00:55_

---

_Label `question` added by @zanieb on 2025-09-18 00:55_

---

_Comment by @zanieb on 2025-09-18 00:57_

`uv pip list` finds the non-virtual environment by default

```
root@bc08349b4fb1:/# uv pip list -v
DEBUG uv 0.8.18
DEBUG Searching for default Python interpreter in virtual environments, managed installations, or search path
DEBUG Searching for managed installations at `/root/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.13.7-linux-aarch64-gnu`
DEBUG Found `cpython-3.13.7-linux-aarch64-gnu` at `/root/.local/share/uv/python/cpython-3.13.7-linux-aarch64-gnu/bin/python3.13` (managed installations)
Using Python 3.13.7 environment at: /root/.local/share/uv/python/cpython-3.13.7-linux-aarch64-gnu
Package Version
------- -------
pip     24.3.1
```

while `uv pip install` is not allowed to find that environment because it's a mutable operation. I can see how that's a bit confusing. I'm not sure if there's a great way to address it though. Most people won't encounter that because the virtual environment interpreter won't be on their `PATH` without activating the environment.

---

_Comment by @dvrogozh on 2025-09-18 01:21_

> I'd recommend not doing that :). Perhaps you meant `uv venv ~/.local/venv`?

No, I meant exactly `~/.local`. Eventually I figured out that's better not to do that. Unfortunately in my case this also means that better not to use `uv`.

For the background, I don't need virtual environment at all as I am working inside the container for ci. Environment is already isolated and adding another layer of isolation which requires to source environment on each step does not quite make sense. From the other hand I also don't want to install dependencies system wide (with `pip install --system`) for various reasons one of which being that this would require `sudo` as in my case container user is not `root`. This lead to the consideration whether `uv` supports installation to `~/.local/` similar to regular `pip`. Here it occurs that `uv` does not support `--user` - there are few issues already filed and closed on that. But in one of the issues which dismissed to introduce `--user` someone suggested a workaround, see https://github.com/astral-sh/uv/issues/2077#issuecomment-1971579880, which was to create environment in `~/.local`. So, I did not come myself to try this out, but just tried the approach. Eventually it does not quite work.

Note that `uv` also has quite strange environment variable `UV_PROJECT_ENVIRONMENT` which is not supported in some commands including `uv pip`. While not directly related to the above issue, it does give another layer of confusion and complexity especially for someone who tries things out.

> I'd recommend not doing that :).

I am totally fine with that. However, can this be translated into the fix in `uv` to prohibit such scenario?

---

_Comment by @konstin on 2025-09-18 07:37_

Unfortunately, a venv in `~/.local` and a `--user` install work very differently. While I agree that a venv in docker is some sort of double-isolation, venvs do have additional benefits, see e.g. https://hynek.me/articles/docker-virtualenv/. A venv gives you the same experience and paths in and out of docker; maybe it's easier to see the `.venv` folder as a build artifact rather than an isolation tool: It's a "bundle" of all your dependencies. venvs are also useful for multi-stage builds, you can populate one in a build stage and then don't need to have uv in the run stage. In the Dockerfile, you can set `VIRTUAL_ENV` and `PATH` to activate the venv, so you don't need to source the environment.

---

_Comment by @zanieb on 2025-09-18 12:57_

I think `uv pip list` and `uv pip show` listing the managed environment is actually a bug. I tested on 0.7.x and it worked as you'd expect. I think this is a regression from https://github.com/astral-sh/uv/pull/7934  that I missed in https://github.com/astral-sh/uv/pull/15061

---
