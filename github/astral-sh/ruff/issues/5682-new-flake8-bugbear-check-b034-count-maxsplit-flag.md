---
number: 5682
title: "New flake8-bugbear check B034: count/maxsplit/flag should be passed as keyword parameters"
type: issue
state: closed
author: jakkdl
labels: []
assignees: []
created_at: 2023-07-11T10:46:38Z
updated_at: 2023-07-11T13:09:50Z
url: https://github.com/astral-sh/ruff/issues/5682
synced_at: 2026-01-07T13:12:15-06:00
---

# New flake8-bugbear check B034: count/maxsplit/flag should be passed as keyword parameters

---

_Issue opened by @jakkdl on 2023-07-11 10:46_

I added a new check to flake8-bugbear, and it would be great to have it incorporated into ruff as well. Copying from the issue https://github.com/PyCQA/flake8-bugbear/issues/397

### Context
* https://github.com/python/cpython/issues/99918#issuecomment-1333382582
* https://github.com/python/cpython/issues/56166

### Example code
```python
$ re.sub('a', 'b', 'aaa')
'bbb'
$ re.sub('a', 'b', 'aaa', re.IGNORECASE)
'bba'
>>> re.split(' ', 'a a a a')
['a', 'a', 'a', 'a']
>>> re.split(' ', 'a a a a', re.I)
['a', 'a', 'a a']
>>> re.split(' ', 'a a a a', flags=re.I)
['a', 'a', 'a', 'a']
```

The fourth parameter to `re.sub[n]` is in fact `count`, and the third to `split` is `maxsplit`, and as seen in the above linked issues this is a *very* common mistake.

### Links
See the issue https://github.com/PyCQA/flake8-bugbear/issues/397 for discussion around implementation, and https://github.com/PyCQA/flake8-bugbear/pull/398 for the PR I wrote implementing it.



---

_Comment by @tjkuson on 2023-07-11 12:58_

I believe this was implemented last night! #5669

It should be in version 0.0.278.

---

_Comment by @jakkdl on 2023-07-11 13:09_

Whoa, so fast! I didn't even consider checking whether it was already implemented, I'll make sure to do that next time. Thanks! :rocket: :rocket: 

---

_Closed by @jakkdl on 2023-07-11 13:09_

---
