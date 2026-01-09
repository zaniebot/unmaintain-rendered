---
number: 14047
title: "UP044 false positive around PEP 692 TypedDict `**kwargs` type hints"
type: issue
state: closed
author: kkom
labels:
  - bug
  - preview
assignees: []
created_at: 2024-11-01T16:34:51Z
updated_at: 2024-11-02T17:10:57Z
url: https://github.com/astral-sh/ruff/issues/14047
synced_at: 2026-01-07T13:12:16-06:00
---

# UP044 false positive around PEP 692 TypedDict `**kwargs` type hints

---

_Issue opened by @kkom on 2024-11-01 16:34_

Since ruff 0.7.2 the code below:

```python
class KwargsDict(TypedDict):
    foo: int
    bar: int

def fun(
    **kwargs: Unpack[KwargsDict],
) -> None: ...
```

Triggers this error:

```
UP044 [*] Use `*` for unpacking
```

I don't think the star syntax is allowed in this case though, see: https://peps.python.org/pep-0692/#introducing-a-new-syntax

And making this change triggers another Ruff error:

```
SyntaxError: Starred expression cannot be used here Ruff
```

---

_Comment by @MichaReiser on 2024-11-01 17:04_

Thanks for reporting this and linking to the relevant PEP section. @diceroll123 are you interested in submitting a fix for the new rule?

---

_Label `bug` added by @MichaReiser on 2024-11-01 17:04_

---

_Label `rule` added by @MichaReiser on 2024-11-01 17:04_

---

_Label `preview` added by @MichaReiser on 2024-11-01 17:04_

---

_Label `rule` removed by @MichaReiser on 2024-11-01 17:04_

---

_Referenced in [astral-sh/ruff#13988](../../astral-sh/ruff/pulls/13988.md) on 2024-11-01 21:18_

---

_Referenced in [astral-sh/ruff#14053](../../astral-sh/ruff/pulls/14053.md) on 2024-11-01 22:26_

---

_Closed by @charliermarsh on 2024-11-02 17:10_

---

_Closed by @charliermarsh on 2024-11-02 17:10_

---

_Referenced in [astral-sh/ruff#14088](../../astral-sh/ruff/issues/14088.md) on 2024-11-04 11:22_

---
