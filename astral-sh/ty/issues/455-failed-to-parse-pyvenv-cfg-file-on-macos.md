```yaml
number: 455
title: Failed to parse pyvenv.cfg file on MacOS
type: issue
state: open
author: Skylion007
labels:
  - imports
assignees: []
created_at: 2025-05-19T17:05:12Z
updated_at: 2025-11-14T14:43:04Z
url: https://github.com/astral-sh/ty/issues/455
synced_at: 2026-01-12T15:54:23Z
```

# Failed to parse pyvenv.cfg file on MacOS

---

_@Skylion007_

### Summary

`ty 0.0.1-alpha.5 (3b726d87a 2025-05-17)`

Contents of pyvenv.cfg
```
home = /usr/local/Cellar/python@3.9/3.9.1_8/Frameworks/Python.framework/Versions/3.9
implementation = CPython
version_info = 3.9.1.final.0
virtualenv = 20.4.2
include-system-site-packages = false
base-prefix = /usr/local/Cellar/python@3.9/3.9.1_8/Frameworks/Python.framework/Versions/3.9
base-exec-prefix = /usr/local/Cellar/python@3.9/3.9.1_8/Frameworks/Python.framework/Versions/3.9
base-executable = /usr/local/opt/python@3.9/bin/python3.9
```

logs:
```
2025-05-19 13:02:29.541067 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
2025-05-19 13:02:29.691439 DEBUG Version: 0.0.1-alpha.5 (3b726d87a 2025-05-17)
2025-05-19 13:02:29.691521 DEBUG Architecture: x86_64, OS: macos, case-sensitive: case-insensitive
2025-05-19 13:02:29.691559 DEBUG Searching for a project in '/Users/agokaslan/git/pytorch'
2025-05-19 13:02:29.691944 DEBUG Resolving requires-python constraint: `>=3.9`
2025-05-19 13:02:29.69197 DEBUG Resolved requires-python constraint to: 3.9
2025-05-19 13:02:29.692004 DEBUG Project without `tool.ty` section: '/Users/agokaslan/git/pytorch'
2025-05-19 13:02:29.69202 DEBUG Searching for a user-level configuration at `/Users/agokaslan/.config/ty/ty.toml`
2025-05-19 13:02:29.692044 INFO Defaulting to python-platform `darwin`
2025-05-19 13:02:29.692062 INFO Python version: Python 3.9, platform: darwin
2025-05-19 13:02:29.692079 DEBUG Adding first-party search path '/Users/agokaslan/git/pytorch'
2025-05-19 13:02:29.692094 DEBUG Using vendored stdlib
2025-05-19 13:02:29.692741 DEBUG Discovering site-packages paths from sys-prefix `/Users/agokaslan/venvs/ai_habitat_brew_py3` (`VIRTUAL_ENV` environment variable')
2025-05-19 13:02:29.692805 DEBUG Attempting to parse virtual environment metadata at '/Users/agokaslan/venvs/ai_habitat_brew_py3/pyvenv.cfg'
```

Error:
```
ty failed
  Cause: Invalid search path settings
  Cause: Failed to discover the site-packages directory: Failed to parse the pyvenv.cfg file at /Users/agokaslan/venvs/ai_habitat_brew_py3/pyvenv.cfg because the following error was encountered when trying to resolve the `home` value to a directory on disk: No such file or directory (os error 2)
```

Tis a shame because the previous version worked great too.


### Version

_No response_

---

_Renamed from "Failed to parse pyvenv.cfg file on Mac" to "Failed to parse pyvenv.cfg file on MacOS" by @Skylion007 on 2025-05-19 17:06_

---

_Label `imports` added by @AlexWaygood on 2025-05-19 17:07_

---

_Comment by @MichaReiser on 2025-05-19 17:11_

Does the path `/usr/local/Cellar/python@3.9/3.9.1_8/Frameworks/Python.framework/Versions/3.9` exist?

---

_Comment by @MichaReiser on 2025-05-19 17:11_

Given that we don't use that value. I think it would be okay for ty to pick up the path and instead print a warning

---

_Comment by @AlexWaygood on 2025-05-19 17:15_

IIRC MacOS system builds do some slightly cursed things with symlinks, which might be confusing us...

> Tis a shame because the previous version worked great too.

I'm somewhat confused here because I don't think we've changed anything that would result in different behaviour here. Are you sure you were setting the `VIRTUAL_ENV` variable when you ran previous versions of ty, @Skylion007? (It's implicitly set whenever you activate a virtual environment in your shell.)

---

_Comment by @Skylion007 on 2025-05-19 17:45_

> Does the path `/usr/local/Cellar/python@3.9/3.9.1_8/Frameworks/Python.framework/Versions/3.9` exist?

Surprisingly no lol, the venv works though.

---

_Comment by @Skylion007 on 2025-05-19 17:46_

Yeah everything wasn't upgraded to 3.9.22 under the hood at some point and the venv global packages did not get updated

---

_Comment by @Skylion007 on 2025-05-19 17:47_

> IIRC MacOS system builds do some slightly cursed things with symlinks, which might be confusing us...
> 
> > Tis a shame because the previous version worked great too.
> 
> I'm somewhat confused here because I don't think we've changed anything that would result in different behaviour here. Are you sure you were setting the `VIRTUAL_ENV` variable when you ran previous versions of ty, [@Skylion007](https://github.com/Skylion007)? (It's implicitly set whenever you activate a virtual environment in your shell.)

This PR looks like it could be culprit? https://github.com/astral-sh/ruff/pull/17529

---

_Added to milestone `Stable` by @carljm on 2025-11-14 14:43_

---
