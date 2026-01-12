```yaml
number: 1626
title: "Keep editable \".\" install when locking and syncing"
type: issue
state: closed
author: PetterS
labels:
  - question
assignees: []
created_at: 2024-02-18T06:57:37Z
updated_at: 2025-01-30T16:39:07Z
url: https://github.com/astral-sh/uv/issues/1626
synced_at: 2026-01-12T15:58:30Z
```

# Keep editable "." install when locking and syncing

---

_@PetterS_

I saw a few other issues tagged with **question**, so I thought it could be OK so a ask one like this.

I have a codebase with some dependencies and some code. I would like to create a venv where all of that is available.

So far `uv` works if I do this:
```
uv pip compile pyproject.toml -o requirements.txt
...
uv pip sync requirements.txt
uv pip install -e .
```
I would like to not have to run the last command after every sync. How can I somehow specify the editable install of the current project in pyproject?

- In poetry, I used a `packages` key under `[tool.poetry]` but that is obviously not going to work here.
- Adding `"myproject @ ."` as a dependency results in a `relative path without a working directory: .` message from `uv`

PS the speed of `uv` looks absolutely fantastic!

---

_Comment by @zanieb on 2024-02-18 08:20_

👋 It looks like you're looking for the behavior described in

- https://github.com/astral-sh/uv/pull/1000

Let me know if that works for you!

---

_Label `question` added by @zanieb on 2024-02-18 08:20_

---

_Comment by @PetterS on 2024-02-18 09:54_

You mean a dependency like `"my-project @ file://."`? That also gives the "relative path without a working directory" error

---

_Comment by @zanieb on 2024-02-18 17:07_

Can you share the full output with the `-v` flag?

---

_Comment by @charliermarsh on 2024-02-18 17:45_

It’s possible that we parse pyproject.toml without passing the relative dir down. I can take a look this week to understand what the correct behavior is and whether we’re matching it.

---

_Comment by @charliermarsh on 2024-02-20 02:23_

One option is that you should be able to do `my-project @ file://${PROJECT_ROOT}`. Do you mind trying that?

---

_Comment by @PetterS on 2024-02-20 18:59_

> my-project @ file://${PROJECT_ROOT}

I then get `error: my-project 1 depends on itself`

---

_Comment by @PetterS on 2024-02-20 19:03_

> > my-project @ file://${PROJECT_ROOT}
> 
> I then get `error: my-project 1 depends on itself`

Update: that worked when I installed the latest version of `uv`. Great!

---

_Comment by @serjflint on 2024-02-20 21:16_

> One option is that you should be able to do `my-project @ file://${PROJECT_ROOT}`. Do you mind trying that?

Hello! ''my-sub-project @ file://${PROJECT_ROOT}/../relative/path#egg=sub_package'' worked, but only halfway:
`error: Distribution not found at: file:///resolved/path%23egg=sub_package`

---

_Comment by @debugger24 on 2024-02-22 19:37_

Hi, I am trying out something similar, using ${PROJECT_ROOT} does work but when we have chain of libraries, it doesn't compile, it take relative path of my current project directly even of dependent libraries.

https://stackoverflow.com/questions/78042589/pip-requirements-for-relative-packages

---

_Comment by @charliermarsh on 2024-03-08 20:59_

I think we intentionally don't allow relative paths in `pyproject.toml` because it's not part of the PEP standard, and that file needs to follow standards, whereas `requirements.txt` is _not_ standardized and already does a few things that don't adhere to them.

---

_Comment by @PetterS on 2024-03-23 09:18_

`my-project @ file://${PROJECT_ROOT}` does not install `my-project` as editable in 0.1.24. It makes a copy. In fact, it seems difficult to ever upgrade it without deleting the `.venv`. `pip compile` and `pip sync` does not change anything

The local project is copied into `.venv/lib/python3.10/site-packages` and then never changed

---

_Comment by @charliermarsh on 2024-03-23 12:30_

You need to install with -e in order to install as editable. For example, “uv pip install -e .”

---

_Comment by @PetterS on 2024-03-23 21:54_

Yes, that works. Would be nice to somehow specify that the project itself should be installed editable. But maybe the pyproject syntax does not allow this

---

_Closed by @charliermarsh on 2024-04-04 03:57_

---

_Comment by @atomiechen on 2024-10-04 18:46_

I found a way to solve this: by specifying `setuptools` as the build system, `uv sync` will make the project editable installed:


```toml
[build-system]
requires = ["setuptools"]
build-backend = "setuptools.build_meta"
```

I verified that `uv.lock` is updated with `source = { editable = "." }` under your project package.

---

_Comment by @joaoprocopio on 2024-10-08 04:01_

thanks @atomiechen!!!!!!!!!

---

_Comment by @robert-elles on 2024-12-16 13:51_

Leads to errors for me, first i get:
```ValueError: invalid pyproject.toml config: `project`.
configuration error: `project` must not contain {'package-mode'} properties```

After removing the line `package-mode = false`, I then get the following error:

```
[stderr]
error: Multiple top-level packages discovered in a flat-layout: ['data', 'logs', 'typings', ...].

To avoid accidental inclusion of unwanted files or directories,
setuptools will not proceed with this build.

If you are trying to create a single distribution with multiple packages
on purpose, you should not rely on automatic discovery.
Instead, consider the following options:

1. set up custom discovery (`find` directive with `include` or `exclude`)
2. use a `src-layout`
3. explicitly set `py_modules` or `packages` with a list of names

To find more information, look for "package discovery" on setuptools docs.
```

---

_Comment by @MrMegaMango on 2024-12-28 12:05_

local module installed by `uv add --editable ./pkg` doesn't work when running `uv run service/server.py`. ModuleNotFoundError: No module named 'pkg'


---

_Comment by @CorentinJ on 2025-01-30 15:15_

> I found a way to solve this: by specifying `setuptools` as the build system, `uv sync` will make the project editable installed:
> 
> [build-system]
> requires = ["setuptools"]
> build-backend = "setuptools.build_meta"
> I verified that `uv.lock` is updated with `source = { editable = "." }` under your project package.

Oh wow I've been trying for so long to make my repository importable as a package outside of its cwd. I tried many variations of `uv add --editable . --dev` but it never worked. This was the solution... that was really far from obvious

---

_Comment by @charliermarsh on 2025-01-30 16:39_

The impact of the build system is covered in the docs: https://docs.astral.sh/uv/concepts/projects/init/.

---
