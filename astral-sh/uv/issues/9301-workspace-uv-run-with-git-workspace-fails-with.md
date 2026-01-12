```yaml
number: 9301
title: "[workspace] `uv run --with=git+workspace` fails with `Build backend failed to determine requirements`"
type: issue
state: open
author: PhilipVinc
labels:
  - question
assignees: []
created_at: 2024-11-21T00:48:28Z
updated_at: 2024-11-23T17:08:44Z
url: https://github.com/astral-sh/uv/issues/9301
synced_at: 2026-01-12T15:59:47Z
```

# [workspace] `uv run --with=git+workspace` fails with `Build backend failed to determine requirements`

---

_@PhilipVinc_

See the error below. 

I'm unsure what the error means

```python
❯ uv run --with git+https://github.com/PhilipVinc/uvtest2 python
 Updated https://github.com/PhilipVinc/uvtest2 (d5efb81)
   Built netket @ git+https://github.com/PhilipVinc/uvtest2@d5efb812fbf0dd2a1c7cc23802a003bdc1b08409#subdirectory=external/netket
   Built netket-checkpoint @ git+https://github.com/PhilipVinc/uvtest2@d5efb812fbf0dd2a1c7cc23802a003bdc1b08409#subdirectory=packages/netket_checkpoint
   Built deepnets @ git+https://github.com/PhilipVinc/uvtest2@d5efb812fbf0dd2a1c7cc23802a003bdc1b08409#subdirectory=packages/deepnets
   Built netket-pro @ git+https://github.com/PhilipVinc/uvtest2@d5efb812fbf0dd2a1c7cc23802a003bdc1b08409#subdirectory=packages/netket_pro
  × Failed to download and build `omnia @ git+https://github.com/PhilipVinc/uvtest2@d5efb812fbf0dd2a1c7cc23802a003bdc1b08409`
  ╰─▶ Build backend failed to determine requirements with `build_wheel()` (exit status: 1)

      [stderr]
      error: Multiple top-level packages discovered in a flat-layout: ['config', 'packages', 'external', 'containers'].

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

_Comment by @charliermarsh on 2024-11-21 04:28_

This appears to be a setuptools error. It's not specific to uv. Is your project structured properly? Can you build it with `python -m build`?

---

_Label `question` added by @charliermarsh on 2024-11-21 04:28_

---

_Comment by @PhilipVinc on 2024-11-21 10:49_

I am trying to add a workspace as a dependency to a project. This works locally, but it does not work if I add it from git.

Compare

```bash
# This works
git clone https://github.com/PhilipVinc/uvtest2
mkdir test && cd test
uv init
uv add ../uvtest2
```

against
```bash
mkdir test2 && cd test2
uv init
uv add git+https://github.com/PhilipVinc/uvtest2
```

which fails with the error above

---

_Comment by @PhilipVinc on 2024-11-21 10:49_

I cannot build the project because it's a workspace, not a python package.

---

_Comment by @charliermarsh on 2024-11-22 03:12_

Yeah, I don't know if this should work. We respect "virtual projects" (or non-package projects) locally, in workspaces, but I don't think we can treat arbitrary dependencies that way. It would break any (real) package that relies on an implicit build backend.

---

_Comment by @PhilipVinc on 2024-11-23 17:08_

@charliermarsh  I'm not sure I understand what an implicit build backend is... 

Overall, I think it's confusing that you can add a local workspace but you cannot add it from git as a dependency. And off the top of my head I see no reason why that should not work.  

Is there some way I could convince you that this is reasonable? Could you elaborate on what makes you doubt this could work? A uv workspace cannot be built (does not declare a `build-system` in pyproject.toml). If there is no `setup.py`, there is no way this project can be built, so if `uv.project` is dexlared I see no other way that this could be treated?

--

The reason I wanted to use this is because if we have a workspace which we use as a workspace for ML projects, when using it in production we want to declare the dependency on it. This bypasses the need to package it or to setup containers (which in HPC we cannot use). 
uv would make it very easy to run a file in the workspace as `uv run --with git+https://ourrepo/ file.py` or more generally just add it as a dependency.



---
