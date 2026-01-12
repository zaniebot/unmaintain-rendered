```yaml
number: 1764
title: Support dynamic import library such as gstreamer python binding
type: issue
state: open
author: hermeschen1116
labels:
  - wish
  - library
assignees: []
created_at: 2025-12-05T06:42:57Z
updated_at: 2025-12-06T04:52:10Z
url: https://github.com/astral-sh/ty/issues/1764
synced_at: 2026-01-12T15:54:25Z
```

# Support dynamic import library such as gstreamer python binding

---

_@hermeschen1116_

### Summary

My project using gstreamer python binding under pygobject. And it uses dynamic import and ty always throws out unresolve-import.

I import the gstreamer lib as below, ty can only resolve gi.

```python
from __future__ import annotations


import gi


gi.require_version("Gst", "1.0")
from gi.repository import Gst  


Gst.init(None)
```

And the diagnostics

```shell
INFO Defaulting to python-platform `darwin`
INFO Python version: Python 3.12, platform: darwin
INFO Indexed 1 file(s) in 0.004s
error[unresolved-import]: Module `gi.repository` has no member `Gst`
  --> src/stream_server/internal/stream/_GStreamer.py:15:27
   |
14 | gi.require_version("Gst", "1.0")
15 | from gi.repository import Gst
   |                           ^^^
   |
info: rule `unresolved-import` is enabled by default

Found 1 diagnostic
```

More specific

```shell
2025-12-05 14:41:50.24026 DEBUG Version: 0.0.1-alpha.31 (51c73d687 2025-12-04)
2025-12-05 14:41:50.246054 DEBUG Architecture: aarch64, OS: macos, case-sensitive: case-insensitive
2025-12-05 14:41:50.246275 DEBUG Searching for a project in '/Users/hermeschen/Repo/work/stream-server'
2025-12-05 14:41:50.248244 DEBUG Resolving requires-python constraint: `>=3.12`
2025-12-05 14:41:50.248307 DEBUG Resolved requires-python constraint to: 3.12
2025-12-05 14:41:50.248328 DEBUG Project without `tool.ty` section: '/Users/hermeschen/Repo/work/stream-server'
2025-12-05 14:41:50.248345 DEBUG Searching for a user-level configuration at `/Users/hermeschen/.config/ty/ty.toml`
2025-12-05 14:41:50.252741 INFO Defaulting to python-platform `darwin`
2025-12-05 14:41:50.252925 DEBUG Resolving `VIRTUAL_ENV` environment variable: /Users/hermeschen/Repo/work/stream-server/.venv
2025-12-05 14:41:50.252973 DEBUG Attempting to parse virtual environment metadata at '/Users/hermeschen/Repo/work/stream-server/.venv/pyvenv.cfg'
2025-12-05 14:41:50.253426 DEBUG Searching for site-packages directory in sys.prefix /Users/hermeschen/Repo/work/stream-server/.venv
2025-12-05 14:41:50.253803 DEBUG Resolved site-packages directories for this virtual environment are: ["/Users/hermeschen/Repo/work/stream-server/.venv/lib/python3.12/site-packages"]
2025-12-05 14:41:50.254202 DEBUG Searching for real stdlib directory in sys.prefix /Users/hermeschen/.local/share/uv/python/cpython-3.12.12-macos-aarch64-none
2025-12-05 14:41:50.254209 DEBUG Resolved real stdlib path for this virtual environment is: /Users/hermeschen/.local/share/uv/python/cpython-3.12.12-macos-aarch64-none/lib/python3.12
2025-12-05 14:41:50.254214 DEBUG Including `.` and `./src` in `environment.root` because a `./src` directory exists
2025-12-05 14:41:50.254424 DEBUG Adding first-party search path `/Users/hermeschen/Repo/work/stream-server/src`
2025-12-05 14:41:50.254428 DEBUG Adding first-party search path `/Users/hermeschen/Repo/work/stream-server`
2025-12-05 14:41:50.254432 DEBUG Using vendored stdlib
2025-12-05 14:41:50.25609 DEBUG Adding site-packages search path `/Users/hermeschen/Repo/work/stream-server/.venv/lib/python3.12/site-packages`
2025-12-05 14:41:50.256109 INFO Python version: Python 3.12, platform: darwin
2025-12-05 14:41:50.256525 DEBUG Adding new file root '/Users/hermeschen/Repo/work/stream-server/.venv/lib/python3.12/site-packages' of kind LibrarySearchPath
2025-12-05 14:41:50.259387 DEBUG Adding new file root '/Users/hermeschen/Repo/work/stream-server' of kind Project
2025-12-05 14:41:50.260043 DEBUG Setting included paths: 1
2025-12-05 14:41:50.260047 DEBUG Reloading files for project `stream-server`
2025-12-05 14:41:50.260082 DEBUG Starting main loop
2025-12-05 14:41:50.260087 DEBUG Waiting for next main loop message.
2025-12-05 14:41:50.260151 DEBUG Checking all files in project 'stream-server'
2025-12-05 14:41:50.265314 INFO Indexed 1 file(s) in 0.005s
2025-12-05 14:41:50.266975 DEBUG Checking file '/Users/hermeschen/Repo/work/stream-server/src/stream_server/internal/stream/_GStreamer.py'
2025-12-05 14:41:50.287359 DEBUG Resolving dynamic module resolution paths
2025-12-05 14:41:50.309481 DEBUG Module `gi.repository.Gst` not found in search paths
2025-12-05 14:41:50.309511 DEBUG Module `gi.repository.Gst` not found while looking in parent dirs
2025-12-05 14:41:50.317814 DEBUG Checking all files took 0.052s
```



### Version

ty 0.0.1-alpha.31 (51c73d687 2025-12-04)

---

_Comment by @carljm on 2025-12-05 23:43_

Thanks for the report! This is a limitation shared by all Python type checkers. Currently the only solution available for this in the Python typing ecosystem is for the library to provide stubs explicitly listing the attributes that should be considered available. There really is no solution for dynamically considering an attribute available only after `gi.require_version(...)` has been called for it. So this would require us building special-case support for pygobject into ty. That's not totally out of the question, but it's unlikely to be a near-term priority. At the moment, a type-ignore in your own code (or providing your own type stub that lists the submodules matching your own `require_version` calls) are the best available options.

---

_Label `library` added by @carljm on 2025-12-05 23:43_

---

_Label `wish` added by @carljm on 2025-12-05 23:43_

---

_Comment by @hermeschen1116 on 2025-12-06 04:52_

I see. Thank you for the response. It'll be quite helpful if Ty can support sub modules under gi for better development experience.
But as what you said, it seems that need help from pyobject maintainers to have stub files included in library?

---
