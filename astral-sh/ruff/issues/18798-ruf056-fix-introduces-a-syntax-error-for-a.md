```yaml
number: 18798
title: RUF056 fix introduces a syntax error for a parenthesized value
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-06-19T17:44:19Z
updated_at: 2025-06-20T21:24:42Z
url: https://github.com/astral-sh/ruff/issues/18798
synced_at: 2026-01-10T11:09:58Z
```

# RUF056 fix introduces a syntax error for a parenthesized value

---

_Issue opened by @dscorbett on 2025-06-19 17:44_

### Summary

The fix for [`falsy-dict-get-fallback` (RUF056)](https://docs.astral.sh/ruff/rules/falsy-dict-get-fallback/) introduces a syntax error when the falsy value is parenthesized.

```console
$ cat >ruf056.py <<'# EOF'
d = {}
not d.get("key", (False))
# EOF

$ ruff --isolated check ruf056.py --select RUF056 --preview --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```

### Version

ruff 0.12.0 (87f0feb21 2025-06-17)

---

_Comment by @MichaReiser on 2025-06-20 06:29_

Result of applying the fix:

```py
d = {}
not d.get("key"))
```

---

_Label `bug` added by @MichaReiser on 2025-06-20 06:29_

---

_Label `fixes` added by @MichaReiser on 2025-06-20 06:29_

---

_Closed by @AlexWaygood on 2025-06-20 21:24_

---
