---
number: 18712
title: PLE0241 fix should be unsafe
type: issue
state: open
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-06-16T14:43:57Z
updated_at: 2025-06-17T05:46:15Z
url: https://github.com/astral-sh/ruff/issues/18712
synced_at: 2026-01-10T01:23:00Z
---

# PLE0241 fix should be unsafe

---

_Issue opened by @dscorbett on 2025-06-16 14:43_

### Summary

The fix for [`duplicate-bases` (PLE0241)](https://docs.astral.sh/ruff/rules/duplicate-bases/) should be marked as unsafe because it changes behavior.
```console
$ cat >ple0241.py <<'# EOF'
class C(int, int): ...
# EOF

$ python ple0241.py 2>&1 | tail -n 1
TypeError: duplicate base class int

$ ruff --isolated check ple0241.py --select PLE0241 --fix
Found 1 error (1 fixed, 0 remaining).

$ python ple0241.py; echo $?
0
```

### Version

ruff 0.11.13 (5faf72a4d 2025-06-05)

---

_Label `fixes` added by @AlexWaygood on 2025-06-16 14:46_

---

_Label `bug` added by @AlexWaygood on 2025-06-16 14:46_

---

_Comment by @dylwil3 on 2025-06-16 17:09_

It's a little unfortunate in this case because I think a lot of the PLE rules relate to runtime errors - so presumably the whole point of turning them on and running `--fix` would be to fix them, no?

---

_Comment by @MichaReiser on 2025-06-16 17:14_

Yeah, I think this works as intended.  I don't think this behavior change is surprising to users. We may want to update our fix safety documentation to make it clear that semantic changes are allowed if they fix a hard error and the user intentions are clear from the code.

---

_Comment by @dscorbett on 2025-06-16 18:06_

Clarifying the documentation for what counts as a safe fix would be a fine outcome for this issue. There are many other issues like this which I haven’t been sure whether to report. For completeness, here is an example where the PLE0241 fix changes behavior in a non-erroring case.

```console
$ cat >ple0241_2.py <<'# EOF'
def SolitaryInt(bases):
    if SolitaryInt in bases[bases.index(SolitaryInt) + 1:]:
        return ()
    return (int,)
SolitaryInt.__mro_entries__ = SolitaryInt

class C1(SolitaryInt): ...
print(C1.__bases__)

class C2(SolitaryInt, SolitaryInt): ...
print(C2.__bases__)
# EOF

$ python ple0241_2.py
(<class 'int'>,)
(<class 'object'>,)

$ ruff --isolated check ple0241_2.py --select PLE0241 --fix
Found 1 error (1 fixed, 0 remaining).

$ python ple0241_2.py
(<class 'int'>,)
(<class 'int'>,)
```

---

_Comment by @dscorbett on 2025-06-16 18:18_

On the other hand, is it safe to assume that `class C(X, X)` was meant to be `class C(X)`? Maybe it was meant to be `class C(X, X2)`. The original code crashes helpfully but the fixed code conceals the bug.

---

_Comment by @MichaReiser on 2025-06-17 05:46_

> On the other hand, is it safe to assume that class C(X, X) was meant to be class C(X)? Maybe it was meant to be class C(X, X2). The original code crashes helpfully but the fixed code conceals the bug.

There's a very small chance that this was the case. My thinking here is that they either see the squiggle line in their IDE and can apply the fix or they run the code and it immediately crashes. If it was intended to be `X, X2`, then I suspect the code will still crash, just somewhere else because certain attributes and methods are now missing or your type checker will even tell you. Either way, I don't think we should optimize for the unlikely case that this was what the user intended. 

---
