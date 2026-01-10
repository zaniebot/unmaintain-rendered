```yaml
number: 8693
title: UP032 fix can shadow INT002 violation
type: issue
state: open
author: ThiefMaster
labels:
  - needs-decision
assignees: []
created_at: 2023-11-15T12:39:10Z
updated_at: 2023-11-15T16:47:05Z
url: https://github.com/astral-sh/ruff/issues/8693
synced_at: 2026-01-10T11:09:50Z
```

# UP032 fix can shadow INT002 violation

---

_Issue opened by @ThiefMaster on 2023-11-15 12:39_

### Python file
```py
_('foo {}'.format(bar))
```

### ruff output

```shell
$ ruff --isolated --select INT,UP --preview rufftest/ruff_sample.py
rufftest/ruff_sample.py:1:3: UP032 [*] Use f-string instead of `format` call
rufftest/ruff_sample.py:1:3: INT002 `format` method argument is resolved before function call; consider `_("string %s") % arg`
Found 2 errors.
[*] 1 fixable with the `--fix` option.

$ ruff --isolated --select INT,UP --preview rufftest/ruff_sample.py --diff
--- rufftest/ruff_sample.py
+++ rufftest/ruff_sample.py
@@ -1 +1 @@
-_('foo {}'.format(bar))
+_(f'foo {bar}')

Would fix 1 error.
```

Luckily the fixed version will then trigger INT001, but maybe UP032 could try to detect if it's inside a gettext call and in that case mark the fix as unsafe?

---

_Comment by @ThiefMaster on 2023-11-15 12:45_

Alternatively a fixer for the `INT` violations would do the job. Probably easier to implement than detecting context in `UP032`...

---

_Label `bug` added by @charliermarsh on 2023-11-15 16:45_

---

_Label `bug` removed by @charliermarsh on 2023-11-15 16:46_

---

_Label `needs-decision` added by @charliermarsh on 2023-11-15 16:46_

---

_Comment by @charliermarsh on 2023-11-15 16:47_

Torn on this... It might be surprising that UP032 doesn't trigger in that case, if we were to change it.

---
