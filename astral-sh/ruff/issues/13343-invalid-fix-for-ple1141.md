```yaml
number: 13343
title: Invalid fix for PLE1141
type: issue
state: closed
author: martinlehoux
labels:
  - rule
  - preview
assignees: []
created_at: 2024-09-13T08:13:49Z
updated_at: 2024-10-04T19:22:27Z
url: https://github.com/astral-sh/ruff/issues/13343
synced_at: 2026-01-12T15:54:53Z
```

# Invalid fix for PLE1141

---

_@martinlehoux_

- I think the autofix is not safe
- Ruff version: 0.6.4
- Ruff command: `ruff check --preview --fix`
```py
d: dict[tuple[int, str], bool] = {}
d[1, "a"] = True

for n, c in d:
    print(n, c)
```

autofixed to 

```py
d: dict[tuple[int, str], bool] = {}
d[1, "a"] = True

for n, c in d.items():
    print(n, c)
```

---

_Label `rule` added by @MichaReiser on 2024-09-13 13:22_

---

_Label `preview` added by @MichaReiser on 2024-09-13 13:22_

---

_Comment by @MichaReiser on 2024-09-13 13:25_

To summarize the issue:

* `for n, c in d` iterates over the dictionary keys
* `for n, c in d.items()` iterates over the `key-value` pairs. 

It's questionable if PLE1141 should be raised for your example because the code only iterates over the keys. Pylint had a similar issue a long time ago https://github.com/pylint-dev/pylint/issues/3283


---

_Comment by @martinlehoux on 2024-09-16 07:14_

Yes exactly. But as I understand it, ruff doesn't use type checking, so it would be difficult to say that the key is a tuple.

---

_Comment by @henribru on 2024-10-04 14:40_

It seems like the fix for this should at the very least be marked as unsafe, since in general it will be impossible to determine if the key is a tuple, even if support was added for inferring it in obvious cases.

---

_Closed by @zanieb on 2024-10-04 19:22_

---
