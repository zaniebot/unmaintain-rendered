---
number: 19601
title: SIM101 fix introduces errors by normalizing f-strings
type: issue
state: open
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-07-28T14:27:20Z
updated_at: 2025-07-29T00:21:33Z
url: https://github.com/astral-sh/ruff/issues/19601
synced_at: 2026-01-07T13:12:16-06:00
---

# SIM101 fix introduces errors by normalizing f-strings

---

_Issue opened by @dscorbett on 2025-07-28 14:27_

### Summary

The fix for [`duplicate-isinstance-call` (SIM101)](https://docs.astral.sh/ruff/rules/duplicate-isinstance-call/) can introduce syntax errors when the duplicated expression is an f-string because it normalizes the expression instead of keeping it unchanged. [Example](https://play.ruff.rs/62d86c81-b089-4054-9ef2-e2414476f7ad):
```console
$ cat >sim101_1.py <<'# EOF'
isinstance(f"{(lambda: 0)}", int) or isinstance(f"{(lambda: 0)}", str)
isinstance(f"{0:{(lambda: 0)}}", int) or isinstance(f"{0:{(lambda: 0)}}", str)
isinstance(f"{0:\x22}", int) or isinstance(f"{0:\x22}", str)
isinstance(f"{0:\x7b}", int) or isinstance(f"{0:\x7b}", str)
# EOF

$ ruff --isolated check sim101_1.py --select SIM101 --unsafe-fixes --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```

It can also change the program’s behavior. [Example](https://play.ruff.rs/61bb0edf-9317-4435-aa72-cdd450ff5eeb):
```console
$ cat >sim101_2.py <<'# EOF'
from datetime import datetime
d = datetime.min
class OddMeta(type):
    def __instancecheck__(self, instance):
        return len(instance) % 2
class Odd(metaclass=OddMeta): pass
print(isinstance(f"{d:\x7d}", int) or isinstance(f"{d:\x7d}", Odd))
print(isinstance(f"{d:\x7b \x7d}", int) or isinstance(f"{d:\x7b \x7d}", Odd))
# EOF

$ python sim101_2.py
True
True

$ ruff --isolated check sim101_2.py --select SIM101 --unsafe-fixes --fix
Found 2 errors (2 fixed, 0 remaining).

$ python sim101_2.py
False
False
```

### Version

ruff 0.12.5 (d13228ab8 2025-07-24)

---

_Label `bug` added by @ntBre on 2025-07-28 14:54_

---

_Label `fixes` added by @ntBre on 2025-07-28 14:54_

---

_Comment by @MeGaGiGaGon on 2025-07-28 17:13_

I worked a lot with the code that is causing the problem while fixing #18742 (fixed in #18882), so here's my understanding of this issue:
The issue is not isolated to `SIM101`. This can happen in any rule that uses AST-based re-writing, see #18742 for a (non-comprehensive) list of more rules.

---

For some background, AST-base rule fixes work like this:
- Receive an AST node as input to the rule.
- Modify the node/generate a new node for the fix.
- Output the string conversion of the AST node

The issue comes from the fact that in Ruff's python AST, strings are normalized eagerly. This is why `\x7b` turns into `{` when fixed by any of the AST-based rules, and is also why other odd behaviors like multiline strings turning into `\n`s happens - The round trip from code -> AST -> code loses information about unnecessary escapes/special characters in the string.

For normal strings this is mostly just annoying, but because interpolated strings have special rules, but still have this eager normalization applied, it leads to behavior changes/syntax errors.

The best fix for this would be changing the AST so that strings are not eagerly normalized, or at least have a recoverable original representation for the generator, which would be a big improvement to all AST-based fixes. I don't have the energy/time to do that, but hopefully someone else does.

---

For this specific case, the `\x7b` problems only occur in interpolation format specs and not in non-interpolated string parts because of this replace:
https://github.com/astral-sh/ruff/blob/130d4e113559a97e5c2dae4f80bd6a9429cef68b/crates/ruff_python_codegen/src/generator.rs#L1504

Fixing `not f"\x7b" == 1` as an example, it goes to `f"{1: {{ }" != 1`. It should be fixable by adding in an earlier replace to `\x7b`, since `{{` escapes don't work in format specs.

---

As for the un-parenthesizing problem, I don't have an immediate idea on how to fix it, since I only looked at the f-string related code. There might be some existing facilities to make the generator create parenthesis if the expression is inside an interpolation element, or some new mechanism to pass that information down may have to be created. Unsure.

---

_Comment by @MeGaGiGaGon on 2025-07-29 00:21_

Update: I've tried to fix this by adding in the `"\\x7b"` case to the above replace code in `unparse_interpolated_string_literal_element`, but it doesn't work because the later `UnicodeEscape` causes the `\\` to be double-escaped into `\\\\`, so even fixing the `\x7b` case will probably require a large re-think of `unparse_interpolated_string_literal_element`, since in it's current form it's just copied from the normal string processing code, which I don't understand the choices behind it too well.

---
