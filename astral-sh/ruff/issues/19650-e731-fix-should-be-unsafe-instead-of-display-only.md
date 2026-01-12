```yaml
number: 19650
title: E731 fix should be unsafe instead of display-only more often
type: issue
state: closed
author: dscorbett
labels:
  - fixes
assignees: []
created_at: 2025-07-30T20:58:05Z
updated_at: 2025-08-15T19:09:56Z
url: https://github.com/astral-sh/ruff/issues/19650
synced_at: 2026-01-12T15:54:57Z
```

# E731 fix should be unsafe instead of display-only more often

---

_@dscorbett_

### Summary

The implementation of [`lambda-assignment` (E731)](https://docs.astral.sh/ruff/rules/lambda-assignment/) says:
https://github.com/astral-sh/ruff/blob/c5ac998892a339be0304c7f9e69a5318b371deb8/crates/ruff_linter/src/rules/pycodestyle/rules/lambda_assignment.rs#L108-L109
but that is not true. It doesn’t matter whether a function is defined with `def` or `lambda`. It is still a descriptor and can still be used as an instance method. A particular lambda function might be designed to be used in a static-like way, but even so, it is not a static method, so the fix wouldn’t change its behavior. The fix should therefore be unsafe instead of display-only, except in contexts where there is another reason to make it display-only. [Example](https://play.ruff.rs/ab8fa57a-8d3c-41dc-995f-3e7c40edccf6):

```console
$ cat >e731.py <<'# EOF'
class A:
    def __init__(self, augend): self.augend = augend
    add = lambda self, addend: self.augend + addend
    add2 = lambda augend, addend: augend + addend
try: print(A(1).add(2))
except TypeError as e: print(e)
try: print(A.add2(1, 2))
except TypeError as e: print(e)
# EOF

$ python e731.py
3
3

$ ruff --isolated check e731.py --select E731 --output-format concise -q
e731.py:3:5: E731 Do not assign a `lambda` expression, use a `def`
e731.py:4:5: E731 Do not assign a `lambda` expression, use a `def`
```

The version with the display-only fix applied produces the same output, showing that the fix can be safe as is (or at least safe enough be unsafe instead of display-only).
```console
$ cat >e731_fixed.py <<'# EOF'
class A:
    def __init__(self, augend): self.augend = augend
    def add(self, addend):
        return self.augend + addend
    def add2(augend, addend):
        return augend + addend
try: print(A(1).add(2))
except TypeError as e: print(e)
try: print(A.add2(1, 2))
except TypeError as e: print(e)
# EOF

$ python e731_fixed.py
3
3
```

Making the method static, as suggested by the E731 implementation comment, can change the program’s behavior when the function wasn’t meant to be static, showing that the comment is wrong.
```console
$ cat >e731_static.py <<'# EOF'
class A:
    def __init__(self, augend): self.augend = augend
    @staticmethod
    def add(self, addend):
        return self.augend + addend
    @staticmethod
    def add2(augend, addend):
        return augend + addend
try: print(A(1).add(2))
except TypeError as e: print(e)
try: print(A.add2(1, 2))
except TypeError as e: print(e)
# EOF

$ python e731_static.py
A.add() missing 1 required positional argument: 'addend'
3
```


### Version

ruff 0.12.7 (c5ac99889 2025-07-29)

---

_Label `fixes` added by @ntBre on 2025-07-30 21:42_

---

_Closed by @ntBre on 2025-08-15 19:09_

---
