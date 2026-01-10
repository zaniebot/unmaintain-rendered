```yaml
number: 2936
title: "converting f-strings to normal strings, contents are not correctly preserved  f'{{test}}' == '{test}' != '{{test}}'"
type: issue
state: closed
author: OliverTED
labels:
  - bug
assignees: []
created_at: 2023-02-15T20:18:40Z
updated_at: 2023-02-17T19:27:03Z
url: https://github.com/astral-sh/ruff/issues/2936
synced_at: 2026-01-10T11:09:45Z
```

# converting f-strings to normal strings, contents are not correctly preserved  f'{{test}}' == '{test}' != '{{test}}'

---

_Issue opened by @OliverTED on 2023-02-15 20:18_

The f-string f"{{test}}" without a variable, it is automatically converted to "{{test}}".  

But they are not the same. (See: https://peps.python.org/pep-0498/#escape-sequences ; e.g. f'{{ {4*10} }}' == '{ 40 }')

Expected: f"{{test}}" -> "{test}"

BTW, Thanks for ruff ... !

---

_Label `bug` added by @charliermarsh on 2023-02-15 20:19_

---

_Comment by @charliermarsh on 2023-02-15 20:19_

Thanks for filing!

---

_Comment by @charliermarsh on 2023-02-15 20:22_

(Confirmed, the autofix here just removes the prefix, but doesn't un-escape any curly contents.)

---

_Closed by @charliermarsh on 2023-02-17 19:27_

---
