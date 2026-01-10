---
number: 4466
title: UV upgrades packages inherited with --system-site-packages
type: issue
state: open
author: lkoelman
labels:
  - bug
  - compatibility
assignees: []
created_at: 2024-06-24T11:22:31Z
updated_at: 2025-12-13T17:44:52Z
url: https://github.com/astral-sh/uv/issues/4466
synced_at: 2026-01-10T01:23:38Z
---

# UV upgrades packages inherited with --system-site-packages

---

_Issue opened by @lkoelman on 2024-06-24 11:22_

Hi there, I'm encountering the following behaviour where uv deviates from `pip` and is causing one of our builds to fail: when you create a virtual environment with `--system-site-packages`, and then install a package without specifying a version, UV chooses the upgrade path (install the latest version in the virtual environment), whereas pip just uses the older version inherited from the base environment.

How to reproduce:

```bash
# (in your base python environment, I used conda)
pip install 'numpy==1.27.0'
pip install uv

python -m venv --system-site-packages .venv
source .venv/bin/activate
# install without verison requirement
uv pip install numpy
# ==> this installs numpy==2.0.0

# install using pip, without version requirements
pip install numpy
# ==> this doesn't install anything, expected behaviour
```


---

_Comment by @zanieb on 2024-06-24 13:45_

Hi. Is `--system-site-packages` missing from your reproduction here?

---

_Comment by @charliermarsh on 2024-06-24 14:47_

(I think this is roughly a known problem, since we don't look at the full `sys.path`.)

---

_Comment by @charliermarsh on 2024-06-24 14:47_

(Same as #2500.)

---

_Comment by @lkoelman on 2024-06-24 14:58_

> Hi. Is `--system-site-packages` missing from your reproduction here?

yes it was missing, I updated it in my original post

---

_Label `bug` added by @zanieb on 2024-06-24 15:25_

---

_Label `compatibility` added by @zanieb on 2024-06-24 15:25_

---

_Comment by @zanieb on 2024-06-24 15:26_

Yeah I think the root cause is the same as #2500 — maybe we need a separate issue that tracks that functionality. Otherwise it seems reasonable to keep this open to track this effect.

---

_Comment by @J3ronimo on 2024-06-26 13:19_

I can confirm that the `include-system-site-packages = true` in pyvenv.cfg seems to have no effect. All dependencies get installed again into the venv although they are already satisfied in the base installation.

---

_Comment by @charliermarsh on 2024-06-26 13:23_

It has an effect on `python` itself -- your interpreter will have access to the system site packages. But we don't respect it in `uv`.

---

_Comment by @J3ronimo on 2024-06-26 14:15_

Ah, understood.

EDIT: I now [found](https://github.com/astral-sh/uv/issues/2500#issuecomment-2103751174) it's actually documented to be this way in `uv venv --help` (the `--help` is important, in `-h` you won't see it):

```
--system-site-packages
    Give the virtual environment access to the system site packages directory.

    Unlike `pip`, when a virtual environment is created with `--system-site-packages`, `uv` will _not_ take
    system site packages into account when running commands like `uv pip list` or `uv pip install`. The
    `--system-site-packages` flag will provide the virtual environment with access to the system site packages
    directory at runtime, but it will not affect the behavior of `uv` commands.
```

We're making great use of this "diff install" behavior of pip to minimize venv sizes in our test suite. Each test needs numpy, matplotlib, scipy etc, which are provided through a rich base install (WinPython, similarly Anaconda) and the dependency requirements of the tests are tried to make best use of what's already in there. This way we can typically reduce >200MB full installs to <10MB, which adds up with every test and version thereof.

Is there another feature in uv that would allow this kind of diff install scenario in order save disk space? Do you have plans to add it at some point (or have you already decided not to)?

If I preinstall a `.pth` file in the venv, pointing to the base install (or some shared libs
folder), would uv respect that? 

---

_Comment by @paveldikov on 2024-08-16 16:45_

> If I preinstall a .pth file in the venv, pointing to the base install (or some shared libs
folder), would uv respect that?

Just tried that -- it does not, but IMO it should.

---

_Referenced in [astral-sh/uv#2500](../../astral-sh/uv/issues/2500.md) on 2024-08-16 16:54_

---

_Comment by @chrisrodrigue on 2024-08-16 20:56_

I have a similar use case to @J3ronimo

My org is tied to using vetted Python package distributions such as Anaconda/WinPython in environments that lack access to PyPI.

The general idea is to simultaneously:
- Utilize a base `conda` environment for access to the interpreter (i.e. `python==3.12.4` and the `site-packages` that come bundled with the Anaconda distribution. This is effectively a system python installation with a large number of vetted packages added to it.
- Utilize `uv` with a `pyproject.toml` file as one normally would

IMO, `uv` should be able to detect if `build-system.requires`, `project.dependencies` and `project.optional-dependencies` versions are already satisfied in the environment of the active python interpreter as the default behavior. That way, running `uv sync` will not inefficiently download and install all the dependencies again for an online system (or not fail outright on an offline system). However, I would expect `uv` to be able to install new requirements that aren't in the base environment to the venv.

---

_Comment by @chrisrodrigue on 2024-08-16 21:13_

A cop-out way to get a list of already-satisfied requirements would be to just shell out to `python -m pip list`, but I'm not sure how one would annotate sources in `uv.lock`. Maybe the file system path? (i.e. `C:\ProgramData\anaconda3\Lib\site-packages\<package>`

---

_Referenced in [astral-sh/uv#7358](../../astral-sh/uv/issues/7358.md) on 2024-09-13 12:55_

---

_Comment by @BubuOT on 2024-09-13 13:17_

https://github.com/astral-sh/uv/issues/7358#issuecomment-2348896632 recommended using `uv pip install --system` as a workaround, but that is not an option for us, because the system python environment is on a read-only partition.

---

_Referenced in [home-assistant/core#128214](../../home-assistant/core/issues/128214.md) on 2024-10-14 02:54_

---

_Comment by @awoimbee on 2024-11-21 11:32_

Same as #6880

---

_Comment by @pradyunsg on 2024-12-12 15:44_

Would a PR for this be welcome?

> If I preinstall a `.pth` file in the venv, pointing to the base install (or some shared libs folder), would uv respect that?

Currently, no.

> (I think this is roughly a known problem, since we don't look at the full sys.path.)

This is accurate. #3500 pulled in sys.path from the interpreter but it's not being used at this time.

Looking through the code for this, it looks like the changes needed here are:

- Update the environment lookup logic to use sys.path rather than sysconfig paths, for installed distributions.
- Track whether something is coming from "outside" the environment, and skip uninstalling such packages (something that'd fail in many cases, in case of missing permissions).

Based on taking a quick look at things, these aren't particularly complex and I'd be happy to explore doing both these pieces, if a PR for this would be useful/welcome.

---

_Comment by @charliermarsh on 2024-12-12 16:16_

Definitely welcome. I’m happy to answer questions too.

---

_Referenced in [astral-sh/uv#9849](../../astral-sh/uv/pulls/9849.md) on 2024-12-12 19:14_

---

_Referenced in [astral-sh/uv#6880](../../astral-sh/uv/issues/6880.md) on 2025-01-07 20:13_

---

_Referenced in [astral-sh/uv#11670](../../astral-sh/uv/pulls/11670.md) on 2025-03-05 17:04_

---

_Referenced in [ecmwf/WeatherGenerator#1352](../../ecmwf/WeatherGenerator/issues/1352.md) on 2025-11-25 14:20_

---

_Comment by @jscanvic on 2025-12-12 19:26_

@BubuOT Did you end up finding a workaround for read-only system site-packages?

---

_Comment by @BubuOT on 2025-12-13 17:44_

@jscanvic not really, no, besides just accepting that uv will re-download and reinstall all the packages that are already in the system venv.

---
