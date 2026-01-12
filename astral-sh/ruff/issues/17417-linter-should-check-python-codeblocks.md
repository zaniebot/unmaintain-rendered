```yaml
number: 17417
title: linter should check python codeblocks
type: issue
state: closed
author: DetachHead
labels:
  - docstring
  - wish
  - linter
assignees: []
created_at: 2025-04-16T01:21:50Z
updated_at: 2025-04-16T10:15:54Z
url: https://github.com/astral-sh/ruff/issues/17417
synced_at: 2026-01-12T15:54:55Z
```

# linter should check python codeblocks

---

_@DetachHead_

i just noticed that `ruff format` was not formatting a codeblock in my docstring despite `docstring-code-format` being enabled.

it turns out it was because the code had a syntax error in it, so i guess the formatter just assumed that meant it wasn't python code (even though the codeblock was prefixed with "```py")

it would be nice if ruff's linter could check codeblocks in docstrings as well, which would have detected that syntax error (among other possible issues)

---

_Label `docstring` added by @AlexWaygood on 2025-04-16 10:11_

---

_Label `wish` added by @AlexWaygood on 2025-04-16 10:11_

---

_Label `linter` added by @AlexWaygood on 2025-04-16 10:11_

---

_Comment by @AlexWaygood on 2025-04-16 10:15_

Thanks, we agree this would be nice. I'm going to close this in favour of #3792, however, which has a lot more discussion attached to it!

---

_Closed by @AlexWaygood on 2025-04-16 10:15_

---
