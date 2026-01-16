```yaml
number: 2508
title: "\"ty\" can't find dist-packages when using the system packages (not a venv) on Linux"
type: issue
state: closed
author: a-h-ismail
labels:
  - imports
assignees: []
created_at: 2026-01-15T09:13:48Z
updated_at: 2026-01-16T12:37:57Z
url: https://github.com/astral-sh/ty/issues/2508
synced_at: 2026-01-16T12:56:13Z
```

# "ty" can't find dist-packages when using the system packages (not a venv) on Linux

---

_@a-h-ismail_

### Summary

Hello Maintainers,

I have recently installed `ty` as an alternative to `pyright` for better performance. The problem is that no matter what I tried, `ty` won't find the `site-packages` for the system Python installation.

More information:
`ty` is installed from the official extension on VSCodium. I tried both the bundled `ty` and the one installed using `pipx` on Debian 13 (my host OS). Without any configuration, I get:
```
INFO Version: 0.0.12
INFO Defaulting to python-platform `linux`
ERROR Failed to create project for workspace `file://<path_to_project>`: Failed to discover the site-packages directory: Invalid selected interpreter in your editor `/usr`: Could not find a `site-packages` directory for this Python installation/executable. Falling back to default settings
INFO Defaulting to python-platform `linux`
INFO Python version: Python 3.13, platform: linux
```
When I include the configuration file `ty.toml`:
```
[environment]
extra-paths = ["/usr/lib/python313.zip","/usr/lib/python3.13","/usr/lib/python3.13/lib-dynload","$HOME/.local/lib/python3.13/site-packages","/usr/local/lib/python3.13/dist-packages","/usr/lib/python3/dist-packages"]
python = "/usr/bin/python3.13"
python-version = "3.13"
```
I get:
```
INFO Version: 0.0.12
INFO Defaulting to python-platform `linux`
ERROR Failed to create project for workspace `file://<path_to_project>`: Failed to discover the site-packages directory: Invalid `environment.python` setting

--> Invalid setting in configuration file `<project_root>/ty.toml`
  |
1 | [environment]
2 | extra-paths = ["/usr/lib/python313.zip","/usr/lib/python3.13","/usr/lib/python3.13/lib-dynload","<home>/.local/lib/python3.13/site…
3 | python = "/usr/bin/python3.13"
  |          ^^^^^^^^^^^^^^^^^^^^^ Could not find a `site-packages` directory for this Python installation/executable
4 | python-version = "3.13"
  |
. Falling back to default settings
INFO Defaulting to python-platform `linux`
INFO Python version: Python 3.13, platform: linux
```
Can I somehow give the `site-packages` to `ty` using something similar to `python -c "import sys; print('\n'.join(sys.path))"` for system paths as a workaround for now?

Thank you

### Version

ty 0.0.12

---

_Renamed from "Ty can't find dist-packages when using the system packages (not a venv) on Linux" to ""ty" can't find dist-packages when using the system packages (not a venv) on Linux" by @a-h-ismail on 2026-01-15 10:35_

---

_Label `imports` added by @AlexWaygood on 2026-01-15 15:05_

---

_Comment by @carljm on 2026-01-15 16:35_

Thanks for the report! This is #1323 

---

_Closed by @carljm on 2026-01-15 16:35_

---

_Comment by @a-h-ismail on 2026-01-16 06:06_

@carljm Do you have any workaround for the time being?
I can help you test any patches for Debian.

---

_Comment by @MichaReiser on 2026-01-16 07:49_

@a-h-ismail see https://github.com/astral-sh/ty/issues/1323#issuecomment-3726358944

---

_Comment by @a-h-ismail on 2026-01-16 12:37_

@MichaReiser Thank you. I have tried the `extra-paths` workaround and it didn't work for some reason. However the `PYTHONPATH` did work. I still get an error from "ty Language server" on startup, but the packages are getting recognized.

I got the directories to add to PYTHONPATH by running `python -c "import sys; print(':'.join(sys.path))"` and removing the leading ':' then exporting the remaining text into PYTHONPATH in `.profile`.

---
