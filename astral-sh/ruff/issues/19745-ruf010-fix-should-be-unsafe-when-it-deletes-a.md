```yaml
number: 19745
title: RUF010 fix should be unsafe when it deletes a comment
type: issue
state: open
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-08-04T17:28:06Z
updated_at: 2025-08-05T18:23:10Z
url: https://github.com/astral-sh/ruff/issues/19745
synced_at: 2026-01-10T11:09:59Z
```

# RUF010 fix should be unsafe when it deletes a comment

---

_Issue opened by @dscorbett on 2025-08-04 17:28_

### Summary

The fix for [`explicit-f-string-type-conversion` (RUF010)](https://docs.astral.sh/ruff/rules/explicit-f-string-type-conversion/) should be marked unsafe when it deletes a comment. [Example](https://play.ruff.rs/7a9bae43-15b3-4ef7-a64a-da6363416774):
```console
$ cat >ruf010.py <<'# EOF'
f"{ascii(
    # comment
    1
)}"
# EOF

$ ruff --isolated check ruf010.py --select RUF010 --fix
Found 1 error (1 fixed, 0 remaining).

$ cat ruf010.py 
f"{1!a}"
```

When the modified code includes a comment and the comment is preserved, the fix should continue to be marked safe. [Example](https://play.ruff.rs/42e79949-3f73-4f91-9205-9c2f3b970a9e):
```python
f"{ascii((
    # comment
    1
))}"
```

### Version

ruff 0.12.7 (c5ac99889 2025-07-29)

---

_Label `bug` added by @ntBre on 2025-08-04 21:51_

---

_Label `fixes` added by @ntBre on 2025-08-04 21:51_

---

_Label `help wanted` added by @ntBre on 2025-08-04 21:51_

---

_Comment by @mikeleppane on 2025-08-05 16:13_

I will take a look at this.

---

_Assigned to @mikeleppane by @ntBre on 2025-08-05 18:23_

---
