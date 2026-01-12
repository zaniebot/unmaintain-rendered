```yaml
number: 1178
title: Resolving the virtual environment of uv scripts
type: issue
state: closed
author: PetterS
labels:
  - question
assignees: []
created_at: 2025-09-13T08:49:28Z
updated_at: 2025-09-13T09:34:16Z
url: https://github.com/astral-sh/ty/issues/1178
synced_at: 2026-01-12T15:54:24Z
```

# Resolving the virtual environment of uv scripts

---

_@PetterS_

### Question

`uv` recommends the following setup for simple scripts:
```
#!/usr/bin/env -S uv run --quiet
# /// script
# dependencies = ["colorama==0.4.6"]
# requires-python = ">=3.12"
# ///

import colorama
```

But when checking this with `ty`:
```
$  uvx ty check mytest.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                                                                             error[unresolved-import]: Cannot resolve imported module `colorama`
 --> mytest.py:7:8
  |
5 | # ///
6 |
7 | import colorama
  |        ^^^^^^^^
  |
info: Searched in the following paths during module resolution:
info:   1. /home/petter/source/dotfiles (first-party code)
info:   2. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

Found 1 diagnostic
```

Is it possible to type-check "uv scripts" in some way?

### Version

ty 0.0.1-alpha.20

---

_Label `question` added by @PetterS on 2025-09-13 08:49_

---

_Comment by @AlexWaygood on 2025-09-13 09:34_

Thanks for the issue! Please see #691

---

_Closed by @AlexWaygood on 2025-09-13 09:34_

---
