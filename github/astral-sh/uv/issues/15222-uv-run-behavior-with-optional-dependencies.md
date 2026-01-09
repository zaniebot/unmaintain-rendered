---
number: 15222
title: uv run behavior with optional dependencies
type: issue
state: closed
author: andythomas
labels:
  - question
assignees: []
created_at: 2025-08-11T15:12:00Z
updated_at: 2025-08-12T02:30:08Z
url: https://github.com/astral-sh/uv/issues/15222
synced_at: 2026-01-07T13:12:19-06:00
---

# uv run behavior with optional dependencies

---

_Issue opened by @andythomas on 2025-08-11 15:12_

### Summary

I found a (to me) inconsistent behavior of `uv run` when compared to `uv pip install`. In detail:

In the 'pyproject.toml', I have the following lines:

EDIT: in `[project.optional-dependencies]`!

```toml
spi = [
    "spidev; platform_system == 'Linux' and (platform_machine == 'armv7l' or platform_machine == 'aarch64')",
    "RPi; platform_system == 'Linux' and (platform_machine == 'armv7l' or platform_machine == 'aarch64')",
]
```

so I can chose to install the required drivers when running on a raspberry pi. With `uv pip install ".[spi]"` I get


```
Using Python 3.12.11 environment at: /Users/thomasa/.venv/matr1x
Resolved 187 packages in 331ms
      Built matr1x @ file:///Users/thomasa/SynologyDrive/Github/IFW_software/core_library
Prepared 1 package in 219ms
Uninstalled 1 package in 17ms
Installed 1 package in 4ms
 ~ matr1x==8.0.1 (from file:///Users/thomasa/SynologyDrive/Github/IFW_software/core_library)
```
 
and it works fine even when running on my desktop mac. It seems to skip the packages, just as I would have expected. However, if I run `uv run pytests --verbose` it first installs the package in a venv with (presumably) all optional dependencies checked and fails:

```
warning: `VIRTUAL_ENV=/Users/thomasa/.venv/matr1x` does not match the project environment path `.venv` and will be ignored; use `--active` to target the active environment instead
  × No solution found when resolving dependencies for split (python_full_version >= '3.12' and sys_platform != 'win32'):
  ╰─▶ Because there are no versions of rpi{(platform_machine == 'aarch64' and sys_platform == 'linux') or (platform_machine == 'armv7l'
      and sys_platform == 'linux')} and matr1x[spi] depends on rpi{(platform_machine == 'aarch64' and sys_platform == 'linux') or
      (platform_machine == 'armv7l' and sys_platform == 'linux')}, we can conclude that matr1x[spi]'s requirements are unsatisfiable.
      And because your project requires matr1x[spi], we can conclude that your project's requirements are unsatisfiable.
```

My expectation is that either both fail or both work. Both working and skipping the packages would be my desired (and expected) behavior. 

The current behavior would force me to put all dependencies that have a platform requirement in the 'dependencies' section of the toml.

### Platform

MacOS; Darwin 24.5.0 arm64

### Version

uv 0.8.3 (7e78f54e7 2025-07-24)

### Python version

Python 3.13.5

---

_Label `bug` added by @andythomas on 2025-08-11 15:12_

---

_Comment by @zanieb on 2025-08-11 15:41_

`uv pip` performs a platform-specific resolution for your current environment, while `uv run` performs a universal resolution to generate a lockfile that works for all developers.

Please read https://docs.astral.sh/uv/concepts/resolution/#platform-specific-resolution

---

_Label `bug` removed by @zanieb on 2025-08-11 15:41_

---

_Label `question` added by @zanieb on 2025-08-11 15:41_

---

_Comment by @andythomas on 2025-08-11 16:25_

Thank you for the insanely quick answer :)

It still feels inconsistent to me. Couldn't uv come up with the following universal resolution:`spi = [ "spidev",  "RPi"]` for the RasPi and `spi = []` for all other systems, because that would happen if I would run `uv pip install` on these systems. Or is there other (edge) cases where this behavior would be undesirable?




---

_Comment by @zanieb on 2025-08-11 16:31_

So... digging into it a little more. The error says

> there are no versions of rpi{(platform_machine == 'aarch64' and sys_platform == 'linux') or (platform_machine == 'armv7l' and sys_platform == 'linux')} 

Maybe a dumb question, but... where is `rpi` coming from here? It's not on PyPI? https://pypi.org/project/rpi/

---

_Comment by @andythomas on 2025-08-11 16:40_

I see now, that is embarrassing. It cannot resolve that for the RasPi platform, because even for that there is no package on PyPi.  It is a typo and presumable means `RPi.GPIO`. With that I did `uv pip compile --universal --extra=spi pyproject.toml
` and it works! I apologize and appreciate the time you took.



---

_Closed by @andythomas on 2025-08-11 16:40_

---

_Comment by @zanieb on 2025-08-12 02:30_

No problem, thanks for following up.

This is a good example of https://github.com/astral-sh/uv/issues/15092 where the markers make this error message more confusing than it should be.

---
