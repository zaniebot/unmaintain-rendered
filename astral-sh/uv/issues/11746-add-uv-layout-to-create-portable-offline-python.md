---
number: 11746
title: "Add `uv layout` to create portable offline Python distributions"
type: issue
state: open
author: chrisrodrigue
labels:
  - enhancement
assignees: []
created_at: 2025-02-24T16:39:10Z
updated_at: 2025-12-31T22:09:49Z
url: https://github.com/astral-sh/uv/issues/11746
synced_at: 2026-01-10T01:25:09Z
---

# Add `uv layout` to create portable offline Python distributions

---

_Issue opened by @chrisrodrigue on 2025-02-24 16:39_

# Proposal

uv could be used as a Python _distribution_ manager to create and use a set of trusted interpreters and packages in airgapped environments.

Inspiration for this comes from [Visual Studio](https://learn.microsoft.com/en-us/visualstudio/install/create-an-offline-installation-of-visual-studio?view=vs-2022#step-2---create-a-local-layout), [Python for Windows](https://docs.python.org/3/using/windows.html#installing-without-ui), and other tools that allow users to create offline layouts.

The goals are similar to [WinPython](https://winpython.github.io/#overview), [Anaconda](https://docs.anaconda.com/anaconda/release-notes/), and [ActiveState](http://activestate.com/platform/supported-languages/python/).

# Example

You have a `pyproject.toml` or `requirements.txt` that declares 1000 packages for a distribution named birdhouse.

From the `pyproject.toml` root, run

```console
uv layout --platform windows
```
Or, if there is no project, run  

```console
uv layout --platform windows --requirements requirements.txt
```

A directory is made that contains uv, Python, and packages for the specified platform. The directory might have a `uv.toml` with various settings for uv to work portably from that directory with the included Python interpreter(s) and packages. Multiple Python versions and package versions might be included. 

This directory can be compressed and distributed to others. The directory would contain everything that albatross, parrot, and canary devs would need to hit the ground running in an airgapped environment. 

Devs can use commands like `uv add`, `uv sync` and `uv run` in any projects on the system without depending on a network connection to access an external package index or external Python interpreter(s). This is a fully offline and portable Python distribution.

# Use Case

Offline systems may not have access to any form of package index. These systems may have multiple users that need to make isolated Python projects containing virtual environments with installed packages. Users may not be authorized to bring in an interpreter or packages, so a distribution containing an interpreter and packages somewhere on the disk is needed.

# Semantics

*Python* means a build of the Python interpreter with all supporting files.

*Packages* means bdists (wheels) or sdists.

*Distribution* means *Python* and *Packages* combined and configured to work together.

*Portable* means to work reliably from a single directory without depending on elevated privileges or pre-configured environment variables, and without breaking if the directory location is changed or renamed.

---

_Label `enhancement` added by @chrisrodrigue on 2025-02-24 16:39_

---

_Comment by @samypr100 on 2025-02-25 02:05_

Is this perhaps an alternative proposal to https://github.com/astral-sh/uv/issues/5802?

---

_Comment by @chrisrodrigue on 2025-02-25 12:38_

> Is this perhaps an alternative proposal to [#5802](https://github.com/astral-sh/uv/issues/5802)?

#5802 is slightly different.

I think the intent of `uv bundle` is to be able to combine an app, its dependencies, and Python for non-developer end users. It would produce an executable binary, likely having a GUI or CLI. This would enable devs to distribute their apps without relying on users to know what uv, Python, PyPI, or virtual environments are.

The intent of `uv layout` ([inspiration](https://learn.microsoft.com/en-us/visualstudio/install/create-an-offline-installation-of-visual-studio?view=vs-2022#step-2---create-a-local-layout)) is to be able to combine uv, packages, and Python for developers. It would produce a directory, likely having a `uv.toml` to configure uv to work with that directory layout when invoked. This would enable devs to create projects without relying on an internet connection.

Both of these commands would be valuable, with `uv bundle` artifacts targeted at users and `uv layout` artifacts targeted at developers. Both commands could read a `pyproject.toml` to produce the desired artifact for the current platform by default, or take any number of `--platform <target-triple>` options (or `--platform all`) to produce artifacts for different platforms.

---

_Renamed from "uv as a Python distribution manager" to "Add `uv layout` to create an offline Python distribution" by @chrisrodrigue on 2025-02-25 12:47_

---

_Renamed from "Add `uv layout` to create an offline Python distribution" to "Add `uv layout` to create offline Python distributions" by @chrisrodrigue on 2025-02-25 12:47_

---

_Referenced in [astral-sh/uv#5802](../../astral-sh/uv/issues/5802.md) on 2025-02-25 14:11_

---

_Renamed from "Add `uv layout` to create offline Python distributions" to "Add `uv layout` to create portable offline Python distributions" by @chrisrodrigue on 2025-02-25 21:01_

---

_Referenced in [astral-sh/uv#11856](../../astral-sh/uv/issues/11856.md) on 2025-02-28 14:49_

---

_Comment by @detrin on 2025-03-05 13:26_

Not gonna lie, this would be wholesome.

---

_Referenced in [astral-sh/uv#2103](../../astral-sh/uv/issues/2103.md) on 2025-03-06 15:10_

---

_Referenced in [astral-sh/uv#11996](../../astral-sh/uv/issues/11996.md) on 2025-03-06 15:12_

---

_Referenced in [astral-sh/uv#12008](../../astral-sh/uv/issues/12008.md) on 2025-03-06 15:16_

---

_Comment by @HernandoR on 2025-03-07 07:01_

Could  a layout include manually solved packages? For my use case there's a venv that needs the following cmd to setup:
```
uv sync
uv pip install pkg_a
uv pip install pkg_b
```
This problem mainly due to the incorrect dependency tagged by pkg_a or its dependence. Nevertheless, i need to isolate it from the uv sync solve, also manually patch the broken pkg_b

A similar situation is for mmcv users, who were recommended to use mim to install mmcv dependencies:
```
uv sync
mim install mmcv mmdet
```



---

_Comment by @chrisrodrigue on 2025-03-07 14:29_

@HernandoR 

This would be a concern for the project scope rather than the layout/distribution scope. 

A layout might have a flat directory (let’s call it `pypackages` for now) containing wheels or sdists, which could potentially have multiple versions of packages to support the needs of various projects on a system. 

This flat directory would serve as an index for uv to source packages from. uv would read the `pyproject.toml` of an individual project on the systems to perform package resolution and install the dependencies from `pypackages` that project’s venv.

For handling packages that have incorrect dependencies, you may want to check out [Dependency metadata](https://docs.astral.sh/uv/concepts/resolution/#dependency-metadata) and [Dependency overrides](https://docs.astral.sh/uv/concepts/resolution/#dependency-overrides).

---

_Comment by @etiennebonnafoux on 2025-03-07 14:39_

> For handling packages that have incorrect dependencies you may want to check out [Dependency metadata](https://docs.astral.sh/uv/concepts/resolution/#dependency-metadata) and [Dependency overrides](https://docs.astral.sh/uv/concepts/resolution/#dependency-overrides).

Does this dependencis metadata override also work for python ? 
something like
 
```toml
[tool.uv.sources]
python = { git = "https://local.gitlab.com/python/python-build-standalone", tag = "v3.12" }
```

---

_Comment by @chrisrodrigue on 2025-03-07 14:49_

> > For handling packages that have incorrect dependencies you may want to check out [Dependency metadata](https://docs.astral.sh/uv/concepts/resolution/#dependency-metadata) and [Dependency overrides](https://docs.astral.sh/uv/concepts/resolution/#dependency-overrides).
> 
> Does this dependencis metadata override also work for python ? something like
> 
> [tool.uv.sources]
> python = { git = "https://local.gitlab.com/python/python-build-standalone", tag = "v3.12" }

See https://github.com/astral-sh/uv/issues/8015

---

_Referenced in [astral-sh/uv#8015](../../astral-sh/uv/issues/8015.md) on 2025-03-14 18:07_

---

_Referenced in [astral-sh/uv#12403](../../astral-sh/uv/issues/12403.md) on 2025-03-24 13:16_

---

_Comment by @chrisrodrigue on 2025-04-10 23:32_

**Nota Bene**: The Python for Windows installer supports a [`/layout` command](https://docs.python.org/3/using/windows.html#installing-without-ui), which pre-downloads all components that the installer would normally download on the fly.

---

_Comment by @yshvrdhn on 2025-12-31 22:09_

any updates here ? 

---
