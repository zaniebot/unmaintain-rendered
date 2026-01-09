---
number: 5737
title: "Allow setting virtual environment options in `pyproject.toml`"
type: issue
state: open
author: chrisrodrigue
labels:
  - configuration
assignees: []
created_at: 2024-08-02T21:06:48Z
updated_at: 2025-06-13T12:44:01Z
url: https://github.com/astral-sh/uv/issues/5737
synced_at: 2026-01-07T13:12:17-06:00
---

# Allow setting virtual environment options in `pyproject.toml`

---

_Issue opened by @chrisrodrigue on 2024-08-02 21:06_

When running `uv venv`, the user may specify the options `--system-site-packages` and `--relocatable`. These values persist in `.venv/pyenv.cfg` as shown:

```txt
home = C:\WinPython\python-3.12.4.amd64
implementation = CPython
uv = 0.2.33
version_info = 3.12.4
include-system-site-packages = true
relocatable = true
```

The `uv sync` command implicitly creates a virtual environment in `.venv`, which is awesome. However, it is not possible to pass these options to `uv sync` or set them in the `tool.uv` section (or a `tool.uv.venv` section) of  `pyproject.toml`. The virtual environment created with `uv sync` always defaults to setting `include-system-site-package` and `relocatable` to `false`.

I propose that `uv` be capable of reading these from `pyproject.toml`, and also when passed to `uv sync`.

Maybe a generalized approach to file-based configuration (both `uv.toml` and `pyproject.toml`) of `uv` behavior could be to support a section corresponding to each `uv` command for all available options:

Example `pyproject.toml` demonstrating command customization:
```toml
[tool.uv]
no-build-isolation = true
dev-dependencies = []

[uv.tool.venv]
relocatable = true
system-site-packages = true

[uv.tool.sync]
relocatable = true
system-site-packages = true
all-extras = true

# Generalization
[uv.tool.command]
option = value
```

---

_Label `configuration` added by @charliermarsh on 2024-08-02 21:08_

---

_Comment by @charliermarsh on 2024-08-02 21:08_

This seems reasonable to me though want a second opinion before adding any configuration.

---

_Comment by @chrisrodrigue on 2024-08-02 21:18_

A more elegant and concise alternative (similar to `pdm` as shown here - [passing constant arguments to every pdm invocation](https://pdm-project.org/en/latest/usage/config/#passing-constant-arguments-to-every-pdm-invocation)):
```toml
[tool.uv.options]
venv = ["--relocatable", "--system-site-packages"]
```



---

_Referenced in [astral-sh/uv#5811](../../astral-sh/uv/issues/5811.md) on 2024-08-06 10:48_

---

_Referenced in [astral-sh/uv#7931](../../astral-sh/uv/issues/7931.md) on 2024-10-05 01:45_

---

_Comment by @zanieb on 2024-10-05 14:51_

I'm kind of hesitant, though I see the benefit. I think it adds a lot of complexity to the configuration and implementation to allow settings to be defined per-command too.

---

_Comment by @ssbarnea on 2024-12-10 17:40_

I am still trying to find where I can enable the `seed` option inside the config as it is causing lots of compatibility issues with lots of tools.

---

_Comment by @zanieb on 2024-12-10 18:13_

@ssbarnea please provide more details, what kind of compatibility issues with what tools? Concrete examples are needed to help motivate the change.

---

_Comment by @RmStorm on 2024-12-16 15:12_

I have a problem with this as well. Basically if the venv gets created by `uv`, the flags in `pyvenv.cfg` are wrong and the code doesn't work. I need apt installed packages on raspberry for the pins to be available in python so `uv add` or `pip install` will yield a broken setup. This means that I must run `uv venv --system-site-packacges .venv` before I can run the code like so: `uv run code.py`. It would be a lot easier to transfer ownership of this to my less software inclined colleagues if they could just `uv run` things!

---

_Referenced in [astral-sh/uv#9996](../../astral-sh/uv/issues/9996.md) on 2025-01-10 17:42_

---

_Referenced in [astral-sh/uv#12429](../../astral-sh/uv/issues/12429.md) on 2025-04-01 07:53_

---

_Comment by @woutervh on 2025-05-07 22:08_

uv 0.7.3

Given a python-project and a pyproject.toml,
is there currently a way to reliably always create the venv with --relocatable?  

I. e. without manually creating the venv and typing command-line options each time?
(I did not find any other way in the docs)

IMHO this would indeed be a nice syntax:

```
...
[uv.tool.venv]
relocatable = true
...

```



---

_Referenced in [astral-sh/uv#11852](../../astral-sh/uv/issues/11852.md) on 2025-05-14 18:08_

---

_Referenced in [astral-sh/uv#13994](../../astral-sh/uv/issues/13994.md) on 2025-06-12 13:38_

---

_Referenced in [astral-sh/uv#9755](../../astral-sh/uv/issues/9755.md) on 2025-06-29 13:48_

---

_Referenced in [astral-sh/uv#15934](../../astral-sh/uv/issues/15934.md) on 2025-12-11 18:29_

---
