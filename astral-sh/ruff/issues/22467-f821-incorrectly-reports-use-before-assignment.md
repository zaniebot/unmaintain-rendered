```yaml
number: 22467
title: "`F821`: Incorrectly reports use before assignment for conditional import"
type: issue
state: open
author: k4lizen
labels:
  - diagnostics
assignees: []
created_at: 2026-01-08T23:39:25Z
updated_at: 2026-01-12T22:04:41Z
url: https://github.com/astral-sh/ruff/issues/22467
synced_at: 2026-01-12T22:24:39Z
```

# `F821`: Incorrectly reports use before assignment for conditional import

---

_@k4lizen_

### Summary

I do this in my code:
```python
from __future__ import annotations

import pwndbg


def myfunc() -> None:
    # note that dbg is an object in the pwndbg module
    if pwndbg.dbg.is_gdblib_available():
        import pwndbg.gdblib.functions
        _ = pwndbg.gdblib.functions.functions
        print("yes!")
    else:
        print("no!")

myfunc()
```

And I get the following:
```
~❯ uv run ruff check repro.py
F823 Local variable `pwndbg` referenced before assignment
 --> repro.py:7:8
  |
6 | def myfunc() -> None:
7 |     if pwndbg.dbg.is_gdblib_available():
  |        ^^^^^^
8 |         import pwndbg.gdblib.functions
9 |         _ = pwndbg.gdblib.functions.functions
  |

Found 1 error.
```

What is going on?

### Repro

```bash
git clone https://github.com/k4lizen/pwndbg/
cd pwndbg
git checkout 4588a455f5e511b567458ef618eb627788b539d5
uv run --all-groups --all-extras ruff check repro.py
```

### Version

ruff 0.14.10

---

_Label `bug` added by @amyreese on 2026-01-09 00:28_

---

_Renamed from "ruff misinterprets a module as an object" to "`F821`: Incorrectly reports use before assignment for conditional import" by @MichaReiser on 2026-01-09 11:12_

---

_Comment by @ntBre on 2026-01-12 20:23_

Thanks for the report! As pointed out on the PR (https://github.com/astral-sh/ruff/pull/22472#issuecomment-3732861947), this code raises an `UnboundLocalError` at runtime, so it seems like Ruff is giving an accurate diagnostic. Is there a reason you think the error should be suppressed?

I cloned pwndbg and tried this to confirm:

```shell
git clone https://github.com/pwndbg/pwndbg.git
cd pwndbg/
uv run python <<EOF
from __future__ import annotations

import pwndbg


def myfunc() -> None:
    # note that dbg is an object in the pwndbg module
    if pwndbg.dbg.is_gdblib_available():
        import pwndbg.gdblib.functions
        _ = pwndbg.gdblib.functions.functions
        print("yes!")
    else:
        print("no!")

myfunc()
EOF
```
```
Traceback (most recent call last):
  File "<stdin>", line 15, in <module>
  File "<stdin>", line 8, in myfunc
UnboundLocalError: cannot access local variable 'pwndbg' where it is not associated with a value
```

This was surprising to me too, but the local `import pwndbg...` makes the `pwndbg` reference within the function a local variable reference, which is then used before its definition. There's even a [Python FAQ](https://docs.python.org/3/faq/programming.html#why-am-i-getting-an-unboundlocalerror-when-the-variable-has-a-value) entry on this topic, although they use an assignment rather than an import in the example.

---

_Comment by @k4lizen on 2026-01-12 21:32_

ou damn my bad, I thought this was fine at runtime. if theres a way for ruff to detect this case and maybe point to an explanation of whats going on that would be nice, but in general I'm fine with closing this issue.

i'm not sure what @dscorbett meant by this
> This seems like a bug in the upstream rule.


---

_Comment by @k4lizen on 2026-01-12 21:34_

as an aside, do you know what the valid/intended way is to achieve what i'm trying to do here?

---

_Comment by @ntBre on 2026-01-12 21:55_

No worries at all, this was also confusing to me! I'll change the label to `diagnostics`, this could be an interesting use for a sub-diagnostic if it doesn't add too much complexity to the rule.

I think dscorbett was referring to the PR summary, which compared Ruff's implementation to pyflakes. I don't think you mentioned pyflakes, but the PR author checked the upstream tool to see if it also flagged this code.

> as an aside, do you know what the valid/intended way is to achieve what i'm trying to do here?

I think if you declare `pwndbg` as `global` within the function, you can get the behavior you want:

```diff
 def myfunc() -> None:
+    global pwndbg
     # note that dbg is an object in the pwndbg module
     if pwndbg.dbg.is_gdblib_available():
         import pwndbg.gdblib.functions
```

I'm getting a different attribute error in that case, but I think that should work in general.


---

_Label `bug` removed by @ntBre on 2026-01-12 21:55_

---

_Label `diagnostics` added by @ntBre on 2026-01-12 21:55_

---

_Comment by @k4lizen on 2026-01-12 22:04_

yup, can confirm that works, thanks!

---
