```yaml
number: 7539
title: new rule - unused top level private variable
type: issue
state: open
author: DetachHead
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-09-20T11:08:30Z
updated_at: 2023-10-27T07:51:28Z
url: https://github.com/astral-sh/ruff/issues/7539
synced_at: 2026-01-10T11:09:49Z
```

# new rule - unused top level private variable

---

_Issue opened by @DetachHead on 2023-09-20 11:08_

if a symbol is prefixed with a `_` and is not used in the same file, it should be reasonable to assume it's not being used at all
```py
_foo = 1
def _bar(): ...
```

---

_Label `rule` added by @charliermarsh on 2023-09-20 13:17_

---

_Label `needs-decision` added by @charliermarsh on 2023-09-20 13:17_

---

_Comment by @KotlinIsland on 2023-10-27 07:39_

`_a` means both private and that it is unused. epic!

---

_Comment by @DetachHead on 2023-10-27 07:51_

maybe `_` means intentionally unused for locally scoped symbols, but private for globally scoped symbols

the rule could also catch these scenarios:
```py
class _Foo: # no error because it's used
    a: int # error because it's unused and in a private class
_Foo()

class Foo:
    _a: int # error because it's private and unused
Foo()

def foo(_bar: int): # no error, local scope so it's intentionally unused
    _baz: int # no error, local scope
    qux: int # error: unused (already handled by F842)
```

---
