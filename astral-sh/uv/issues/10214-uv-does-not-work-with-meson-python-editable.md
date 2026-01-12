```yaml
number: 10214
title: uv does not work with meson-python editable installs
type: issue
state: open
author: lucascolley
labels:
  - compatibility
assignees: []
created_at: 2024-12-28T17:31:09Z
updated_at: 2025-02-06T10:14:24Z
url: https://github.com/astral-sh/uv/issues/10214
synced_at: 2026-01-12T16:00:08Z
```

# uv does not work with meson-python editable installs

---

_@lucascolley_

As per https://github.com/astral-sh/uv/pull/7857#issuecomment-2564382110, breaking the discussion from there out into a new issue.

---

A simple reproducer:

```
git clone https://github.com/scikit-learn/scikit-learn.git
cd scikit-learn
uv sync --extra tests
uv run pytest sklearn/linear_model
```

Error:

```
ImportError while loading conftest '/Users/lucascolley/programming/scikit-learn/sklearn/conftest.py'.
.venv/lib/python3.10/site-packages/_scikit_learn_editable_loader.py:311: in find_spec
    tree = self._rebuild()
.venv/lib/python3.10/site-packages/_scikit_learn_editable_loader.py:345: in _rebuild
    subprocess.run(self._build_cmd, cwd=self._build_path, env=env, stdout=subprocess.DEVNULL, check=True)
../../.local/share/uv/python/cpython-3.10.15-macos-aarch64-none/lib/python3.10/subprocess.py:503: in run
    with Popen(*popenargs, **kwargs) as process:
../../.local/share/uv/python/cpython-3.10.15-macos-aarch64-none/lib/python3.10/subprocess.py:971: in __init__
    self._execute_child(args, executable, preexec_fn, close_fds,
../../.local/share/uv/python/cpython-3.10.15-macos-aarch64-none/lib/python3.10/subprocess.py:1863: in _execute_child
    raise child_exception_type(errno_num, err_msg, err_filename)
E   FileNotFoundError: [Errno 2] No such file or directory: '/Users/lucascolley/.cache/uv/builds-v0/.tmpKC37b1/bin/ninja'
```

---

@rgommers summarised the problem:

> For editable installs, build isolation should be disabled for things to work well. This probably requires a bit of context, so here it is:
> 
> Editable installs were not designed well for dealing with packages with compiled code. The pip defaults completely ignore compiled code and assume what setuptools does: force the user to manually rebuild the package with either pip install or python setup.py build_ext -i (which is a bad experience). meson-python is primarily used for projects with compiled code (as is scikit-build-core, which has similar capabilities to meson-python) and hence tries to do better - it will auto-rebuild whenever one touches compiled code so that the experience of working on C/C++/Cython code is very similar to working on pure Python code. For that to happen, one needs to have build dependencies available in the dev env, or at least in a fixed location. For pip, it's up to the user of the editable install to add --no-build-isolation to the install command for things to work as intended with auto-rebuilds. Hopefully uv has the chance to offer a better experience here.

@henryiii said

> If uv didn't use a different random path every time, but cached the build environment, then that would be solved, I think. 

cc @samypr100 @zanieb @bluss 

---

_Label `compatibility` added by @zanieb on 2024-12-28 17:44_

---

_Comment by @samypr100 on 2024-12-28 17:57_

Thanks for opening a dedicated issue for meson-python support in uv 😊

I believe there was [some effort](https://github.com/astral-sh/uv/pull/7857#issuecomment-2412479901) in getting build isolation working with in meson-python such that it works with uv, although I haven't tried it recently to see if there's any new changes.


---

_Comment by @lucascolley on 2025-01-03 17:48_

> I believe there was some effort in getting build isolation working with in meson-python such that it works with uv,

~@eli-schwartz do you know about this?~

---

_Comment by @lucascolley on 2025-01-03 17:49_

> I believe there was some effort in getting build isolation working with in meson-python such that it works with uv,

maybe I should instead be asking @dnicolodi

---

_Comment by @eli-schwartz on 2025-01-03 18:08_

meson-python attempts to rebuild compiled code changes on during interpreter startup, to avoid the case where you create an editable install, modify a C file, and rerun the python interpreter but your changes don't apply because you didn't re-run `pip install -e .`.

Build isolation (a concept with a number of issues that doesn't always work) is entirely unable to cope with this, as the entire point of build isolation is to have build dependencies in a temporary virtual environment which are immediately gotten rid of. So, ninja isn't available at import time and things fail.

It is detected at editable creation and "frozen" using the full path, so as long as the virtual environment exists, even if it is not active, it should be able to rebuild -- but the point of temporary build isolation is that they do get removed, so that is not helpful.

---

_Comment by @eli-schwartz on 2025-01-03 18:12_

I'm not sure how you'd go about solving this problem from the meson-python side. You need to have the same install of meson both inside and outside of the build environment, since meson writes the path into the generated build.ninja. So simply adding it as additional runtime dependencies for editable installs would not be a solution -- and is anyways forbidden by pep 621.

You can install ninja via apt or rpm or portage or pacman, and meson-python will detect and use that from /usr/bin/ninja instead of installing a python wheel of ninja into the virtual environment. That partially ameliorates the issue.

---

_Comment by @lucascolley on 2025-01-03 19:26_

> I believe there was [some effort](https://github.com/astral-sh/uv/pull/7857#issuecomment-2412479901)

okay, so I guess that was referring to @henryiii's:

> Meson-python should also detect a moving build environment and rerun from scratch if it's detected, a feature I recently added to scikit-build-core.

---

_Comment by @eli-schwartz on 2025-01-03 19:42_

How is it possible to rerun from scratch inside the import hook?

Certainly, if you re-run `uv pip install -e .` the build backend can run again, reconfigure from scratch, and correctly build. That will work fine with meson-python as far as I know, and I assume that is what @henryiii means when saying scikit-build-core handles that use case.

But meson-python is re-running the inner loop of the build backend -- it is compiling out-of-date C/C++/Fortran files -- on interpreter startup, the first time you import the editable-install module. It is not able to message `uv` at that time to reinstall the build backend. I don't believe scikit-build-core attempts to handle this case, it assumes when you edit C/C++/Fortran files you re-install the editable package.

---

_Comment by @lucascolley on 2025-02-06 10:13_

For anyone coming to this issue in an attempt to make a meson-python project installable in a project workflow as editable, an alternative is to use https://pixi.sh/latest/reference/pixi_manifest/#no-build-isolation, provided that dependencies are available as conda packages. Pixi installs the conda dependencies first, so they are available when `uv` is called to resolve the PyPI dependencies and during the build process.

For anyone specifically trying to develop scikit-learn with a project workflow, see https://github.com/glemaitre/scikit-learn-workspace/blob/main/src/scikit-learn/pixi.toml for how this works in practice!

---
