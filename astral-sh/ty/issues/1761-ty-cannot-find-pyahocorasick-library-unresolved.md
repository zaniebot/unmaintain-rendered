```yaml
number: 1761
title: "ty cannot find pyahocorasick library [unresolved-import]"
type: issue
state: closed
author: find0x90
labels: []
assignees: []
created_at: 2025-12-04T18:26:33Z
updated_at: 2025-12-04T18:29:13Z
url: https://github.com/astral-sh/ty/issues/1761
synced_at: 2026-01-12T15:54:25Z
```

# ty cannot find pyahocorasick library [unresolved-import]

---

_@find0x90_

### Summary

I'm seeing this issue on macOS 26.1.

```zsh
$ uv init example && cd example
Initialized project `example` at `/private/tmp/example`

$ uv add pyahocorasick
Using CPython 3.14.0
Creating virtual environment at: .venv
Resolved 3 packages in 21ms
Installed 2 packages in 3ms
 + pyahocorasick==2.2.0
 + pyrefly==0.44.1

$ echo "import ahocorasick; print(ahocorasick)" > main.py

$ uv run main.py
<module 'ahocorasick' from '/private/tmp/example/.venv/lib/python3.14/site-packages/ahocorasick.cpython-314-darwin.so'>

$ uvx ty check
Installed 1 package in 2ms
error[unresolved-import]: Cannot resolve imported module `ahocorasick`
 --> main.py:1:8
  |
1 | import ahocorasick; print(ahocorasick)
  |        ^^^^^^^^^^^
  |
info: Searched in the following paths during module resolution:
info:   1. /private/tmp/example (first-party code)
info:   2. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info:   3. /Users/test/.cache/uv/archive-v0/Ch7k4fP1__Z1J0G-0wUY6/lib/python3.9/site-packages (site-packages)
info:   4. /private/tmp/example/.venv/lib/python3.9/site-packages (site-packages)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

Found 1 diagnostic
```

### Version

ty 0.0.1-alpha.31 (51c73d687 2025-12-04)

---

_Comment by @AlexWaygood on 2025-12-04 18:29_

Thanks!

---

_Closed by @AlexWaygood on 2025-12-04 18:29_

---
