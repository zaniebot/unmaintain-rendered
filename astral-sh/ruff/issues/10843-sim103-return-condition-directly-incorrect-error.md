```yaml
number: 10843
title: SIM103 return condition directly incorrect error message
type: issue
state: closed
author: IronCore864
labels:
  - question
assignees: []
created_at: 2024-04-09T02:25:53Z
updated_at: 2024-04-10T04:29:44Z
url: https://github.com/astral-sh/ruff/issues/10843
synced_at: 2026-01-12T15:54:50Z
```

# SIM103 return condition directly incorrect error message

---

_@IronCore864_

I have searched SIM103 in the open issues and didn't find anything related, so creating this one.

It seems the error message/prompt for SIM103 can be incorrect in some cases.

My code base is quite large, but if I copy the single function out for testing, I could not reproduce. It only happens on the large code base:

```bash
lint: commands[0]> ruff check --preview
ops/testing.py:3469:9: SIM103 Return the condition `keys is not None and notice.key not in keys` directly
     |
3467 |           if types is not None and notice.type not in types:
3468 |               return False
3469 |           if keys is not None and notice.key not in keys:
     |  _________^
3470 | |             return False
3471 | |         return True
     | |___________________^ SIM103
     |
     = help: Inline condition

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

I think the prompt/error message is not correct. How could:
```
if condition:
    return False

return True
```
be replaced by `return condition`? The message should be something like "return not condition instead", not "return condition instead". If the user copy/paste the suggestion it'd be wrong.

Only
```
if condition:
    return True

return False
```
can be replaced directly by `return condition` instead.

With latest ruff version 0.3.5.



---

_Renamed from "SIM103 return condition directly bug" to "SIM103 return condition directly incorrect error message" by @IronCore864 on 2024-04-09 02:26_

---

_Comment by @charliermarsh on 2024-04-09 02:44_

In general, you can replace:

```python
if condition:
    return False
return True
```

With:

```python
return not condition
```

So in your case, it could be:

```python
return not (keys is not None and notice.key not in keys)
```

Or, simplified:

```python
return keys is None or notice.key in keys
```


---

_Label `question` added by @charliermarsh on 2024-04-09 02:44_

---

_Comment by @carljm on 2024-04-09 14:32_

I think the OP is pointing out that the diagnostic message sort of implies you can replace the code with `return condition`, whereas actually in this case you need `return not condition`, and the diagnostic doesn't clarify this distinction.

---

_Comment by @charliermarsh on 2024-04-10 02:42_

Makes sense -- I'll invert it.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-10 02:43_

---

_Closed by @charliermarsh on 2024-04-10 04:29_

---
