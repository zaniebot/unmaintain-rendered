```yaml
number: 18508
title: "UP045 fixes `Optional[None]` to `None | None`"
type: issue
state: open
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-06-06T18:14:40Z
updated_at: 2025-06-06T19:27:17Z
url: https://github.com/astral-sh/ruff/issues/18508
synced_at: 2026-01-12T15:54:56Z
```

# UP045 fixes `Optional[None]` to `None | None`

---

_@dscorbett_

### Summary

The fix for [`non-pep604-annotation-optional` (UP045)](https://docs.astral.sh/ruff/rules/non-pep604-annotation-optional/) can produce `None | None`, the persistently inconvenient exception to PEP 604.

```console
$ cat >up045.py <<'# EOF'
from typing import Optional
foo: Optional[None] = None
# EOF

$ ruff --isolated check up045.py --select UP045 --preview --target-version py310 --fix 
Found 1 error (1 fixed, 0 remaining).

$ cat up045.py
from typing import Optional
foo: None | None = None

$ python up045.py 2>&1 | tail -n 1
TypeError: unsupported operand type(s) for |: 'NoneType' and 'NoneType'
```

### Version

ruff 0.11.13 (5faf72a4d 2025-06-05)

---

_Label `bug` added by @AlexWaygood on 2025-06-06 18:28_

---

_Label `fixes` added by @AlexWaygood on 2025-06-06 18:28_

---

_Label `help wanted` added by @AlexWaygood on 2025-06-06 18:28_

---

_Comment by @chirizxc on 2025-06-06 18:59_

we should ignore it or fix `foo: Optional[None] = None` => `foo: None = None`?

---

_Comment by @ntBre on 2025-06-06 19:06_

Is there ever a valid reason for `Optional[None]` or is it a likely mistake on its own? If it's probably a mistake, we should leave it alone in UP045 and possibly consider a separate rule to catch it.

---

_Comment by @chirizxc on 2025-06-06 19:16_

> Is there ever a valid reason for `Optional[None]` or is it a likely mistake on its own? If it's probably a mistake, we should leave it alone in UP045 and possibly consider a separate rule to catch it.

`Optional[None]` => `Union[None, None]`, i don't think that makes sense


---

_Comment by @chirizxc on 2025-06-06 19:22_

should this be put a separate rule? or should the fix just do from `Optional[None]` to `None`?

---

_Comment by @ntBre on 2025-06-06 19:27_

> should this be put a separate rule? or should the fix just do from `Optional[None]` to `None`?

Yeah that's what I was getting at. In my opinion, we should *not* transform `Optional[None]` to `None` because that's assuming a lot about the user's intention when the original code was likely a separate mistake. So the action items would be:
- Avoid offering an UP045 fix here (again because it's unclear what the user wanted)
- Open a separate rule suggestion for catching `Optional[None]` so we can discuss that generally

---
