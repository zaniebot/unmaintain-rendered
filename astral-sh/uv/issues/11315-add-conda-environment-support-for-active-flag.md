---
number: 11315
title: "Add `conda` environment support for `--active` flag"
type: issue
state: open
author: MilkClouds
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2025-02-07T12:04:44Z
updated_at: 2025-09-08T18:11:42Z
url: https://github.com/astral-sh/uv/issues/11315
synced_at: 2026-01-10T01:25:03Z
---

# Add `conda` environment support for `--active` flag

---

_Issue opened by @MilkClouds on 2025-02-07 12:04_

### Summary

The new `--active` flag (https://github.com/astral-sh/uv/pull/11189) is a great addition and I think it goes some way to allow uv and conda to work better together. But it does not respect `conda` environment and only use `VIRTUAL_ENV` variable.

Would it be possible to add a support for `conda` environment for `--active` flag? Simply using `CONDA_PREFIX` instead of `VIRTUAL_ENV` when `CONDA_DEFAULT_ENV` is not `base` and not empty would be enough, I guess.

### Example

_No response_

---

_Label `enhancement` added by @MilkClouds on 2025-02-07 12:04_

---

_Comment by @zanieb on 2025-02-07 15:38_

This seems reasonable, but will require some refactoring to consolidate that logic.

In theory, this also means we'd warn when `CONDA_PREFIX` is set and does not match the project environment. Though we could omit that.

---

_Referenced in [astral-sh/uv#6612](../../astral-sh/uv/issues/6612.md) on 2025-05-18 10:12_

---

_Comment by @RafalSkolasinski on 2025-05-18 18:25_

That'd be a great feature and fill a hole in feature parity with `poetry`, and docs gave me hope it'd already be working:

> 🚀 A single tool to replace pip, pip-tools, pipx, poetry, pyenv, twine, virtualenv, and more.

from: https://docs.astral.sh/uv/#highlights

> Since uv has no dependency on Python, it can install into virtual environments other than its own.

from: https://docs.astral.sh/uv/pip/environments/#using-arbitrary-python-environments

> When running a command that mutates an environment 
> ...
> An activated Conda environment based on the CONDA_PREFIX environment variable.


from: https://docs.astral.sh/uv/pip/environments/#discovery-of-python-environments

If I am not mistaken it was enough for me to 
```bash
VIRTUAL_ENV=$CONDA_PREFIX uv sync --active
```
but it'd be great if it worked out of the box. 

It may be my solution for https://github.com/astral-sh/uv/issues/1495 but I love to install set of locked packages into a conda environment and then be able to activate it from anywhere.

---

_Referenced in [astral-sh/uv#11273](../../astral-sh/uv/issues/11273.md) on 2025-06-06 10:39_

---

_Comment by @RafalSkolasinski on 2025-08-11 22:19_

Inspired by #11273 I created a fish function (alias)
```
function uv
    if set -q CONDA_PREFIX
        env UV_PROJECT_ENVIRONMENT=$CONDA_PREFIX command uv $argv
    else if set -q VIRTUAL_ENV
        env UV_PROJECT_ENVIRONMENT=$VIRTUAL_ENV command uv $argv
    else
        command uv $argv
    end
end
```
that gets me the desired behaviour.

---

_Comment by @dan-blanchard on 2025-09-08 18:06_

This would be amazing. It's so annoying having to specify `UV_PROJECT_ENVIRONMENT=$CONDA_PREFIX` whenever I use `uv` with a conda environment.

---

_Comment by @zanieb on 2025-09-08 18:11_

I'm fine with supporting this if someone wants to work on it. It might not be easy though.

---

_Label `help wanted` added by @zanieb on 2025-09-08 18:11_

---
