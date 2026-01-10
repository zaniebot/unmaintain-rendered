---
number: 16370
title: How to update default python?
type: issue
state: open
author: myarcana
labels:
  - question
assignees: []
created_at: 2025-10-20T09:34:15Z
updated_at: 2025-12-20T15:06:13Z
url: https://github.com/astral-sh/uv/issues/16370
synced_at: 2026-01-10T01:26:05Z
---

# How to update default python?

---

_Issue opened by @myarcana on 2025-10-20 09:34_

### Question

I just did `uv python upgrade` and uv installed python 3.12, but `uv run python --version` is still 3.11.14

How can I make the global python always use the latest python version?

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @myarcana on 2025-10-20 09:34_

---

_Comment by @thijsvandien on 2025-10-22 11:17_

`uv python pin --global 3.12` should write such preference to the user-level `.python-version` file, to then be used unless the working directory or an ancestor has its own.

---

_Comment by @myarcana on 2025-10-22 14:29_

> `uv python pin --global 3.12` should write such preference to the user-level `.python-version` file, to then be used unless the working directory or an ancestor has its own.

where is the user-level .python-version file?

---

_Comment by @thijsvandien on 2025-10-22 16:58_

That depends on your platform/environment. On macOS and Linux it's commonly `~/.config/uv`, and on Windows `%APPDATA%\uv` – see https://docs.astral.sh/uv/concepts/configuration-files/.

---

_Comment by @konstin on 2025-12-16 12:13_

Are you using `uv run python` in a project? In that case it will use the project Python version, not the global Python version.

---

_Comment by @myarcana on 2025-12-18 05:17_

> That depends on your platform/environment. On macOS and Linux it's commonly `~/.config/uv`, and on Windows `%APPDATA%\uv` – see https://docs.astral.sh/uv/concepts/configuration-files/.

`~/.config/uv` does not exist



> Are you using `uv run python` in a project? 

No, even `cd / && uv run python --version` reports Python 3.11.14

---

_Comment by @konstin on 2025-12-18 09:31_

Can you check the verbose logs with `uv run -v python`? This should print why it picks that python version. If that doesn't show any existing configuration file, `uv python pin --global 3.12` can create one.

---

_Comment by @myarcana on 2025-12-20 14:51_

> Can you check the verbose logs with `uv run -v python`? This should print why it picks that python version. If that doesn't show any existing configuration file, `uv python pin --global 3.12` can create one.

```
DEBUG uv 0.9.18 (Homebrew 2025-12-16)
DEBUG Acquired shared lock for `/Users/me/.cache/uv`
DEBUG No project found; searching for Python interpreter
DEBUG No Python version file found in ancestors of working directory: /Users/me/Downloads
DEBUG Using request timeout of 30s
DEBUG Searching for default Python interpreter in virtual environments, managed installations, or search path
DEBUG Searching for managed installations at `/Users/me/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.11.14-macos-aarch64-none`
DEBUG Found `cpython-3.11.14-macos-aarch64-none` at `/Users/me/.local/share/uv/python/cpython-3.11.14-macos-aarch64-none/bin/python3.11` (managed installations)
DEBUG Using Python 3.11.14 interpreter at: /Users/me/.local/share/uv/python/cpython-3.11.14-macos-aarch64-none/bin/python3.11
DEBUG Running `python`
```

```
% ls /Users/me/.local/share/uv/python
.cache/
.gitignore
.lock
.temp/
cpython-3.11.13-macos-aarch64-none/
cpython-3.11.14-macos-aarch64-none/
cpython-3.11.6-macos-aarch64-none/
cpython-3.8.20-macos-aarch64-none/
```


Why does it only have these versions and not Python 3.12?

---

_Comment by @injust on 2025-12-20 15:03_

> I just did `uv python upgrade` and uv installed python 3.12

If you only had Python 3.11 installed, `uv python upgrade` shouldn't have installed Python 3.12.

Can you confirm (`uv python list 3.12`) that Python 3.12 is actually installed? If not, you need to install Python 3.12 first (`uv python install 3.12)`.

---

`uv python upgrade --help` should be made clearer that it only upgrades _to the latest supported patch release_, as stated in https://docs.astral.sh/uv/guides/install-python/#upgrading-python-versions.

---
