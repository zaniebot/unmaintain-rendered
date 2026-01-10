---
number: 1484
title: "How can I integrate `tox` into `uv`?"
type: issue
state: closed
author: paddyroddy
labels:
  - question
assignees: []
created_at: 2024-02-16T12:33:32Z
updated_at: 2024-03-04T01:54:09Z
url: https://github.com/astral-sh/uv/issues/1484
synced_at: 2026-01-10T01:23:07Z
---

# How can I integrate `tox` into `uv`?

---

_Issue opened by @paddyroddy on 2024-02-16 12:33_

I find that `tox` can take a long time to install dependencies (longer than doing it locally), it would be nice to benefit from `uv` for this. Think that the [relevant bit in tox is here](https://github.com/tox-dev/tox/blob/47bcea693845a285c859312904c61d50e8928f0c/src/tox/tox_env/python/pip/pip_install.py#L66).

---

_Label `question` added by @zanieb on 2024-02-16 14:38_

---

_Comment by @atmartinez on 2024-02-16 15:41_

https://tox.wiki/en/latest/config.html#install_command looks to be a possible solution to putting uv in place of pip

---

_Comment by @Czaki on 2024-02-16 16:22_

No, It does not allow as uv is not installed in venv, and you cannot install it when you change install command... 

To help myself with this task, I started implementing tox plugin. https://github.com/Czaki/tox-uv

But I hit the bug in `uv` it do not allow to install with full path to wheel file:

```
py311-macos-pyqt5-no_cov: install_package> python -I -m uv pip install --force-reinstall --no-deps /tmp/.tox/.tmp/package/5/napari-0.5.0a2.dev578+g17b00de2.tar.gz
error: Failed to parse `/tmp/.tox/.tmp/package/5/napari-0.5.0a2.dev578+g17b00de2.tar.gz`
  Caused by: Expected package name starting with an alphanumeric character, found '/'
/tmp/.tox/.tmp/package/5/napari-0.5.0a2.dev578+g17b00de2.tar.gz
```

---

_Comment by @charliermarsh on 2024-02-16 16:23_

@Czaki -- We can install via a full path, we just require a package name right now (like `napari @ /tmp/.tox/.tmp/package/5/napari-0.5.0a2.dev578+g17b00de2.tar.gz`).

---

_Comment by @Czaki on 2024-02-16 16:27_

@charliermarsh Ok. I do not control this part of tox, and it will need to do hacking that may not be stable. Why provide a full path to sdist or wheel is not enough and require extracting it from the file name? 

---

_Comment by @charliermarsh on 2024-02-16 16:30_

It's just not implemented yet. In general, we can't read the package name from sdist files (like the one you linked -- that looks like an sdist?) since it doesn't follow any particular spec. Wheels _do_ follow a spec and so we could read the package name from the filename in that case.

---

_Comment by @Czaki on 2024-02-16 16:32_

`python -m build --sdist` do not follow any spec?

---

_Comment by @Czaki on 2024-02-16 16:45_

Ok. I have created a workaround. It works nice with wheels, but end with error for sdist:

```
⠹ Resolving dependencies...                                                                              thread 'main' panicked at crates/uv-resolver/src/resolution.rs:120:26:
Every package should have metadata
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
py311-macos-pyqt5-no_cov: exit 101 (0.35 seconds) /Users/grzegorzbokota/Documents/Projekty/napari> python -I -m uv pip install --force-reinstall --no-deps 'napari @ /tmp/.tox/.tmp/package/7/napari-0.5.0a2.dev578+g17b00de2.tar.gz' pid=3499
```

---

_Comment by @zanieb on 2024-02-16 16:55_

Related to #313 

---

_Comment by @Czaki on 2024-02-16 17:08_

tox devs published owns tox-uv plugin https://github.com/tox-dev/tox-uv (but it crashes tox in my case)

---

_Referenced in [tox-dev/tox-uv#1](../../tox-dev/tox-uv/issues/1.md) on 2024-02-16 17:18_

---

_Comment by @gaborbernat on 2024-02-16 20:40_

@charliermarsh now that https://pypi.org/project/tox-uv/ is launched, I think we can close this. Unless you want to document it somewhere, the advice to use that. sdists seem to still be troublesome occasionally, but otherwise work well.

---

_Referenced in [astral-sh/uv#1585](../../astral-sh/uv/issues/1585.md) on 2024-02-17 12:14_

---

_Comment by @charliermarsh on 2024-03-04 01:54_

Closing as `tox-uv` now exists!

---

_Closed by @charliermarsh on 2024-03-04 01:54_

---
