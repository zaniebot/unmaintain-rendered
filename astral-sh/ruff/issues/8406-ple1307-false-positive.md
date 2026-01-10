```yaml
number: 8406
title: "PLE1307: false positive"
type: issue
state: closed
author: spaceone
labels:
  - bug
assignees: []
created_at: 2023-11-01T15:46:44Z
updated_at: 2023-11-02T04:36:54Z
url: https://github.com/astral-sh/ruff/issues/8406
synced_at: 2026-01-10T11:09:50Z
```

# PLE1307: false positive

---

_Issue opened by @spaceone on 2023-11-01 15:46_

PLE1307: false positive:
```python
$ cat x.py
print('[%c]' % ('x',))
print('[%c]' % 'x')
$ ruff x.py
x.py:1:7: PLE1307 Format type does not match argument type
x.py:2:7: PLE1307 Format type does not match argument type
Found 2 errors.
```

---

_Comment by @zanieb on 2023-11-01 15:59_

Thanks for the report. I dug into this a bit and the issue is that `%c` is an integer format type. Characters can be coerced to integers so they work at runtime, but Ruff's type analysis e.g. `analyze::type_inference::PythonType` does not support characters. I'm not sure how best to represent objects that can be both characters and strings in our type system so there's not a quick fix for this although it does seem easy enough to detect strings with a single item and special case them.



---

_Label `bug` added by @zanieb on 2023-11-01 16:11_

---

_Closed by @zanieb on 2023-11-02 04:36_

---
