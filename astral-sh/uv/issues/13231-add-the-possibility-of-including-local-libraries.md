```yaml
number: 13231
title: Add the possibility of including local libraries
type: issue
state: open
author: XavierCLL
labels:
  - enhancement
  - needs-decision
assignees: []
created_at: 2025-04-30T14:24:15Z
updated_at: 2025-10-13T12:33:13Z
url: https://github.com/astral-sh/uv/issues/13231
synced_at: 2026-01-12T16:01:22Z
```

# Add the possibility of including local libraries

---

_@XavierCLL_

## Context

For my Python tests, I need to include local Python libraries that are not installable packages (e.g., not available on PyPI or as wheels). Specifically, I’m developing some QGIS plugins, and I need to import the QGIS Python API, which is located at /usr/lib/python3.13/site-packages/qgis in the local QGIS installation. Since there's no official QGIS package available on PyPI, I cannot install it via pip, so I need to include as a local library.

## In Poetry

I can do easily in Poetry like (works perfectly):

```toml
packages = [
    { include = "qgis", from = "/usr/lib/python3.13/site-packages" },
]
```

## In UV

I tried to do that in UV with (I hope UV works like this):

```toml
dependencies = ["qgis"]

[tool.uv.sources]
qgis = { path = "/usr/lib/python3.13/site-packages/qgis"}
```

But UV expects a wheel, project or setup.py file, UV doesn't understand that it's a local Python library:

```bash
  × Failed to build `qgis @ file:///usr/lib/python3.13/site-packages/qgis`
  ╰─▶ /usr/lib/python3.13/site-packages/qgis does not appear to be a Python project, as neither `pyproject.toml` nor `setup.py` are present in the directory
```

After reading the docs and doing some testing, I think there is no, right now, a way to do that in UV. This is an essential feature in my case to migrate to UV.

Thanks

---

_Label `enhancement` added by @XavierCLL on 2025-04-30 14:24_

---

_Comment by @zanieb on 2025-04-30 14:26_

Where is the qgis source? How was it installed into `site-packages`?

---

_Comment by @XavierCLL on 2025-04-30 14:31_

It is always inside the package installation, e.g. for Archlinux https://archlinux.org/packages/extra/x86_64/qgis/ (Package Contents) or in Ubuntu https://packages.ubuntu.com/oracular/amd64/python3-qgis/filelist

But you can never find it independ as a pypi or wheel file it is always together with the software (I guess that happens for more apps not just with Qgis)

---

_Comment by @zanieb on 2025-04-30 17:52_

Interesting, thanks. Even in the source, this isn't a package:

```
❯ wget https://qgis.org/downloads/qgis-3.42.1.tar.bz2
❯ tar -xvf qgis-3.42.1.tar.bz2
./python/PyQt6/core/additions/projectdirtyblocker.py
❯ cd qgis-3.42.1
❯ ls python
3d			PyQt6			console			ext-libs		processing		server			user.py
CMakeLists.txt		__init__.py		core			gui			pyplugin_installer	sipify.yaml		utils.py
PyQt	
```

Instead, they use CMake to install things manually.

I'm not sure if we'll support this, but it is a fair use-case.

---

_Comment by @XavierCLL on 2025-04-30 21:02_

Yeah there are some binaries (.so, .dll) in that Qgis python lib, therefore, it is not completed from source code, it needs to be compiled or installed.

---

_Label `needs-decision` added by @charliermarsh on 2025-05-04 12:51_

---

_Comment by @LoyVanBeek on 2025-10-13 12:33_

I think I have a similar case, where I'm building something with https://fields2cover.github.io/source/installation.html.
This builds a Python package (with SWIG, into and a `.so` file) into the `dist-packages`. Is there any way to at least declare such a dependency? 



---
