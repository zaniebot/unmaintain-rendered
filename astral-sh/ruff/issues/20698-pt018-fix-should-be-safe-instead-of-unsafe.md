```yaml
number: 20698
title: PT018 fix should be safe instead of unsafe generally, and unsafe with comments instead of suppressed
type: issue
state: open
author: dscorbett
labels:
  - fixes
assignees: []
created_at: 2025-10-04T13:07:15Z
updated_at: 2025-10-06T13:14:25Z
url: https://github.com/astral-sh/ruff/issues/20698
synced_at: 2026-01-12T15:54:57Z
```

# PT018 fix should be safe instead of unsafe generally, and unsafe with comments instead of suppressed

---

_@dscorbett_

### Summary

The fix for [`pytest-composite-assertion` (PT018)](https://docs.astral.sh/ruff/rules/pytest-composite-assertion/) is marked unsafe, but it doesn’t change behavior with any combination of truthy, falsy, or exception-raising expressions, so it should be safe. [Example](https://play.ruff.rs/e4bf6235-3d35-40c8-a862-932f8b340810):
```console
$ cat >pt018_1.py <<'# EOF'
try:
    assert True and True
except Exception as e:
    print(f"{type(e).__name__}:{e}")

try:
    assert True and False
except Exception as e:
    print(f"{type(e).__name__}:{e}")

try:
    assert True and ~0j
except Exception as e:
    print(f"{type(e).__name__}:{e}")

try:
    assert False and True
except Exception as e:
    print(f"{type(e).__name__}:{e}")

try:
    assert False and False
except Exception as e:
    print(f"{type(e).__name__}:{e}")

try:
    assert False and ~0j
except Exception as e:
    print(f"{type(e).__name__}:{e}")

try:
    assert ~0j and True
except Exception as e:
    print(f"{type(e).__name__}:{e}")

try:
    assert ~0j and False
except Exception as e:
    print(f"{type(e).__name__}:{e}")

try:
    assert ~0j and ~0j
except Exception as e:
    print(f"{type(e).__name__}:{e}")

try:
    assert not (True or True)
except Exception as e:
    print(f"{type(e).__name__}:{e}")

try:
    assert not (True or False)
except Exception as e:
    print(f"{type(e).__name__}:{e}")

try:
    assert not (True or ~0j)
except Exception as e:
    print(f"{type(e).__name__}:{e}")

try:
    assert not (False or True)
except Exception as e:
    print(f"{type(e).__name__}:{e}")

try:
    assert not (False or False)
except Exception as e:
    print(f"{type(e).__name__}:{e}")

try:
    assert not (False or ~0j)
except Exception as e:
    print(f"{type(e).__name__}:{e}")

try:
    assert not (~0j or True)
except Exception as e:
    print(f"{type(e).__name__}:{e}")

try:
    assert not (~0j or False)
except Exception as e:
    print(f"{type(e).__name__}:{e}")

try:
    assert not (~0j or ~0j)
except Exception as e:
    print(f"{type(e).__name__}:{e}")
# EOF

$ python pt018_1.py | md5sum
78395b16351cee36624f637cee853666  -

$ ruff --isolated check pt018_1.py --select PT018 --unsafe-fixes --fix
Found 18 errors (18 fixed, 0 remaining).

$ python pt018_1.py | md5sum
78395b16351cee36624f637cee853666  -
```

The fix is suppressed when there are comments within the asserted expression, in which case it should instead be marked unsafe as in most other rules. (The fix is already marked unsafe when there is a comment at the end of the statement, which is correct.) [Example](https://play.ruff.rs/90b579c6-03ad-4c61-bedf-7cd10bb5f907):
```console
$ cat >pt018_2.py <<'# EOF'
assert (
    # ...
    True and True
)
assert True and True  # ...
# EOF

$ ruff --isolated check pt018_2.py --select PT018 --unsafe-fixes --fix --output-format concise
pt018_2.py:1:1: PT018 Assertion should be broken down into multiple parts
Found 2 errors (1 fixed, 1 remaining).

$ cat pt018_2.py
assert (
    # ...
    True and True
)
assert True
assert True
```

### Version

ruff 0.13.3 (188c0dce2 2025-10-02)

---

_Label `fixes` added by @ntBre on 2025-10-06 13:14_

---
