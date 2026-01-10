```yaml
number: 2286
title: "`repr` gets incorrect `Literal` types for strings"
type: issue
state: open
author: dscorbett
labels:
  - bug
  - runtime semantics
assignees: []
created_at: 2025-12-31T04:08:52Z
updated_at: 2025-12-31T11:00:36Z
url: https://github.com/astral-sh/ty/issues/2286
synced_at: 2026-01-10T01:56:41Z
```

# `repr` gets incorrect `Literal` types for strings

---

_Issue opened by @dscorbett on 2025-12-31 04:08_

### Summary

ty’s implementation of `repr` for strings doesn’t match Python’s. [Example](https://play.ty.dev/c83eb871-1800-45c1-bb22-584e434c02d3):
```console
$ cat >invalid-assignment.py <<'# EOF'
from typing import Literal
x: Literal["'\"\\x05⦂🙾'"] = repr('"\5\u2982\U0001F67E')
# EOF

$ ty check --output-format concise invalid-assignment.py
invalid-assignment.py:2:29: error[invalid-assignment] Object of type `Literal["'\\\"\\u{5}\\u{2982}\\u{1f67e}'"]` is not assignable to `Literal["'\"\\x05⦂🙾'"]`
Found 1 diagnostic
```

`repr` changes between Python versions to match Unicode. It is not safe to make assumptions about its exact output for strings. Its behavior for some characters might be considered reliable enough.

### Version

ty 0.0.8 (aa7559db8 2025-12-29)

---

_Label `bug` added by @AlexWaygood on 2025-12-31 11:00_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-12-31 11:00_

---
