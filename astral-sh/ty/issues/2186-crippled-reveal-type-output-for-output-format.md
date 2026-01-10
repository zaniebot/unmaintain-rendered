---
number: 2186
title: Crippled reveal_type output for --output-format=gitlab/github
type: issue
state: closed
author: abelcheung
labels:
  - bug
  - diagnostics
assignees: []
created_at: 2025-12-23T15:11:00Z
updated_at: 2025-12-24T15:28:25Z
url: https://github.com/astral-sh/ty/issues/2186
synced_at: 2026-01-10T01:52:52Z
---

# Crippled reveal_type output for --output-format=gitlab/github

---

_Issue opened by @abelcheung on 2025-12-23 15:11_

### Summary

`reveal_type` output works when `--output-format` is set to `full` or `concise`, but not for `github` or `gitlab`. Here is a minimal example:

```python
from typing import reveal_type

x = "str".lower()
reveal_type(1)
reveal_type(x)
```

Concise output produces:

```
ty check --python-version 3.11 --output-format concise test.py
test.py:4:13: info[revealed-type] Revealed type: `Literal[1]`
test.py:5:13: info[revealed-type] Revealed type: `LiteralString`
Found 2 diagnostics
```

However the type is chopped off from `github` or `gitlab` result. For example, this is the output of `github` output format:

```
ty check --python-version 3.11 --output-format github test.py
::notice title=ty (revealed-type),file=[redacted]]\test.py,line=4,col=13,endLine=4,endColumn=14::test.py:4:13: revealed-type: Revealed type
::notice title=ty (revealed-type),file=[redacted]\test.py,line=5,col=13,endLine=5,endColumn=14::test.py:5:13: revealed-type: Revealed type
```

### Version

ty 0.0.5 (d37b7dbd9 2025-12-20)

---

_Label `diagnostics` added by @AlexWaygood on 2025-12-23 15:16_

---

_Added to milestone `M1` by @AlexWaygood on 2025-12-23 15:29_

---

_Label `bug` added by @AlexWaygood on 2025-12-23 15:30_

---

_Comment by @ntBre on 2025-12-23 15:30_

Ah the GitHub and GitLab formats use `Diagnostic::body`, while concise uses `Diagnostic::concise_message`. It doesn't seem to break any Ruff tests to switch them to `concise_message`, so that might do the trick!

---

_Assigned to @ntBre by @ntBre on 2025-12-23 15:31_

---

_Closed by @ntBre on 2025-12-24 15:28_

---
