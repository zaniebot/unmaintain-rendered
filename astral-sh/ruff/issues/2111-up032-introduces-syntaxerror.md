```yaml
number: 2111
title: UP032 introduces SyntaxError
type: issue
state: closed
author: spaceone
labels:
  - bug
assignees: []
created_at: 2023-01-23T18:53:36Z
updated_at: 2023-01-23T19:11:26Z
url: https://github.com/astral-sh/ruff/issues/2111
synced_at: 2026-01-10T11:09:45Z
```

# UP032 introduces SyntaxError

---

_Issue opened by @spaceone on 2023-01-23 18:53_

```
$ cat foo.py
attribute = 'foo'
template = '({}={{0!e}})'.format(attribute)
print(template)
$ python3 foo.py
(foo={0!e})
$ ruff --select UP032 --fix foo.py
$ cat foo.py
attribute = 'foo'
template = f'({attribute}={0!e})'
print(template)
$ python3 foo.py
  File "foo.py", line 2
    template = f'({attribute}={0!e})'
              ^
SyntaxError: f-string: invalid conversion character: expected 's', 'r', or 'a'

```

---

_Comment by @charliermarsh on 2023-01-23 18:56_

Thanks! Taking a look.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-23 18:56_

---

_Label `bug` added by @charliermarsh on 2023-01-23 19:05_

---

_Closed by @charliermarsh on 2023-01-23 19:11_

---
