---
number: 1422
title: Support custom virtual environment directory names
type: issue
state: closed
author: jkomalley
labels:
  - configuration
  - virtualenv
assignees: []
created_at: 2024-02-16T03:58:43Z
updated_at: 2025-12-02T10:49:45Z
url: https://github.com/astral-sh/uv/issues/1422
synced_at: 2026-01-07T13:12:16-06:00
---

# Support custom virtual environment directory names

---

_Issue opened by @jkomalley on 2024-02-16 03:58_

It is common for virtual environment directories to be either .venv/ or venv/, but only .venv/ is currently supported in without needing to activate the environment. Since both are common names for virtual environments, could both be supported by default in uv? And/Or could there potentially be a config/environment variable that holds a user defined default virtual environment directory name that gets checked along with .venv/VIRTUAL_ENV/CONDA_PREFIX?

---

_Label `configuration` added by @charliermarsh on 2024-02-16 04:08_

---

_Comment by @woutervh on 2024-02-17 04:56_

I wonder why any hardcoding names are needed.
Just because a random folder is called ".venv" it is assumed a python virtualenv?

Why not checking  on the presence of "pyvenv.cfg" + "bin" / "lib"-folders?


Why would this need to be different?

```
  > uv venv .venv
  > uv venv foo
```

---

_Comment by @zanieb on 2024-02-17 05:09_

@woutervh We don't want to scan every directory in your current directory on every invocation, so we use a heuristic.

---

_Comment by @woutervh on 2024-02-17 06:49_

@zanieb 

how are you supposed to install a package when using a non-standard venv-name?
.venv is only used in an actual development-project.


```
uv venv foo
cd foo
?
```

---

_Comment by @woutervh on 2024-02-17 07:11_

> We don't want to scan every directory in your current directory on every invocation, .

That is a small performance-hit i'm very happy to endure,
compared to setting an environment every time you switch into a dozen directories.

the python-executable in a virtualenv is symlink to a global python,
yet it takes into account it's relative location to decide it is in a virtualenv and put its site-packages to its pythonpath.
if python can do that, why not uv?

I would love to see a uv-symlink in the venv's bin/  that only operates on the venv.
```
uv venv foo 
cd foo
bin/uv pip install ...
```


---

_Comment by @wakamex on 2024-02-17 08:11_

pyenv-virtualenv solves this by identifying the env to be used in a folder in a `.python-version` file, which is then looked up automatically every time you enter a folder, and auto-activated. I'd love to see that behavior in `uv`, which I describe here: https://github.com/astral-sh/uv/issues/1578.

---

_Comment by @woutervh on 2024-02-18 02:23_

from https://github.com/astral-sh/uv/issues/1495#issuecomment-1950793228

I have a solution for my use-case:  symlink the foo-folder to a .venv-subfolder

```
uv venv foo
cd foo
ln -s . .venv
uv pip install <package>
```

@jkomalley this solves your use-case?
```
ln -s venv  .venv
```


---

_Comment by @jkomalley on 2024-02-18 05:53_

Yes, that does work for my use case. Thanks!

I still think adding "venv/" to the venv search makes sense since (IMO) it's just as common as ".venv/", and both are listed as conventions in the venv [documentation](https://docs.python.org/3/library/venv.html). The default one uv creates can be ".venv", but both should be expected by default since both are so common.

I think an option for venvs with other "non-default" names is to specify them in the pyproject.toml like this:

```toml
[tool.uv]
venvs = ["env1", "env2"]
```

Or, since multiple venvs potentially means different dependencies or python versions for each, maybe something like this:

```toml
[tool.uv.venvs.env1]
python="3.12"
deps=[
    "env1_dep1",
    "env1_dep2"
]

[tool.uv.venvs.env2]
python="3.10"
deps=[
    "env2_dep1",
    "env2_dep2"
]
```
For either, uv could then add "env1" and "env2" to the venv directory search.

When you make a new venv like `uv venv foo`, uv can add `foo` to pyproject.toml.

And when you run `uv venv env1`, and env1 doesn't exist,  uv could create the venv and populate it with the env1 dependencies specified in pyproject.toml.

---

_Renamed from "Support "venv/"  virtual environment directory" to "Support custom virtual environment directory names" by @zanieb on 2024-02-18 08:01_

---

_Referenced in [astral-sh/uv#1625](../../astral-sh/uv/issues/1625.md) on 2024-02-18 08:05_

---

_Comment by @zanieb on 2024-02-18 08:16_

See also:
- https://github.com/astral-sh/uv/issues/1396
- https://github.com/astral-sh/uv/issues/1632

---

_Label `virtualenv` added by @zanieb on 2024-02-18 20:38_

---

_Referenced in [astral-sh/uv#1722](../../astral-sh/uv/issues/1722.md) on 2024-02-19 23:56_

---

_Referenced in [astral-sh/uv#2248](../../astral-sh/uv/issues/2248.md) on 2024-03-06 18:46_

---

_Referenced in [astral-sh/uv#2665](../../astral-sh/uv/pulls/2665.md) on 2024-03-26 14:37_

---

_Comment by @woutervh on 2024-04-02 18:19_

> I have a solution for my use-case: symlink the foo-folder to a .venv-subfolder

>   uv venv foo
>  cd foo
>  ln -s . .venv
>   uv pip install <package>

Unfortunately, I find myself in the situation to support windows-users.
Anno 2024 making symlinks on Windows is still PITA  (Requires Powershell, administrator access, windows development mode)

---

_Comment by @zanieb on 2024-04-02 21:09_

@woutervh I think you should be able to use a junction

---

_Comment by @b-phi on 2024-08-17 14:03_

When running `uv sync` for a workspace, uv doesn't seem to respect `VIRTUAL_ENV` or an active virtual environment. The docs are pretty clear about this
> uv.lock and .venv for the entire workspace are created next to this pyproject.toml.

In this case, being able to configure the name to use somewhere would be very helpful. Any of CLI parameters, pyproject.toml, envvar, etc would work for my use case. 

---

_Comment by @zanieb on 2024-10-09 13:40_

This was sort of accomplished in https://github.com/astral-sh/uv/pull/6834, though it can't be customized outside of projects and the `uv pip` commands won't respect it.

---

_Referenced in [xdslproject/xdsl#3294](../../xdslproject/xdsl/pulls/3294.md) on 2024-12-06 14:53_

---

_Comment by @levzlotnik on 2025-07-18 02:03_

I was trying to work with `uv` and firebase functions, and it's such an amazing coincidence that BOTH don't support specifying the virtualenv directories. `uv` wants `.venv`, firebase wants `venv`, and so now I can't work with `uv` because of firebase functions.
Truly one of the design choices of all time.
EDIT: my bad, I just found out I can use `UV_PROJECT_ENVIRONMENT`. Apologies

---

_Comment by @charliermarsh on 2025-07-18 02:05_

You can use `UV_PROJECT_ENVIRONMENT` to customize the virtual environment if you'd like.

---

_Comment by @levzlotnik on 2025-07-18 02:07_

Thanks, my bad. I didn't find this in the docs

---

_Comment by @smashedr on 2025-11-15 20:50_

> You can use `UV_PROJECT_ENVIRONMENT` to customize the virtual environment if you'd like.

Is there another way to do this? Like maybe in the existing configuration file `pyproject.toml`? I don't want to add the requirement of an environment variable.

---

_Comment by @zanieb on 2025-12-02 10:49_

@smashedr Not at this time, no. Changing the environment name makes it harder to have consistent discovery mechanisms in editors, it's not intended to be a common pattern.

(I'm going to close this as we don't intend to add automated discovery of arbitrary virtual environment names and there are various ways to target other environments already)

---

_Closed by @zanieb on 2025-12-02 10:49_

---
