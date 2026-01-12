```yaml
number: 9857
title: RUF027 false negative on method calls
type: issue
state: closed
author: ocaballeror
labels:
  - bug
assignees: []
created_at: 2024-02-06T11:50:23Z
updated_at: 2024-02-08T15:00:22Z
url: https://github.com/astral-sh/ruff/issues/9857
synced_at: 2026-01-12T15:54:49Z
```

# RUF027 false negative on method calls

---

_@ocaballeror_

From what I read in [the original PR](https://github.com/astral-sh/ruff/pull/9728), the rule is meant to ignore string literals that have an immediate method call on them to avoid false positives, but from what I'm seeing, every line with a method call is completely ignored, even if the method is not invoked on the string itself. Is this intended behavior?

Sample code:
```python
import logging


class Test:
    def method(self, arg):
        pass


name = "john"

print("hello {name}")  # correctly fails
logging.info("hello {name}")  # should fail but doesn't
Test().method("hello {name}")  # also should fail but doesn't
```
Invoked with:
```
ruff --isolated --select RUF027 --preview thing.py
```

Ruff version:
```
ruff 0.2.1
```


---

_Label `question` added by @MichaReiser on 2024-02-06 12:10_

---

_Comment by @MichaReiser on 2024-02-06 12:11_

It seems intentional. This is from the documentation explaining when the rule doesn't trigger.

> /// 3. The literal (or a parent expression of the literal) has a direct method call on it (for example: `"{value}".format(...)`)

@snowsignal can you help us understand the motivation behind it?

---

_Comment by @ocaballeror on 2024-02-06 15:10_

What does a "parent expresssion of the literal" mean exactly? If it means it won't trigger on things like `logging.info("{var}")`, then the value of this rule decreases significantly IMO.

Shouldn't it only count if a method is called on the string itself?

---

_Comment by @snowsignal on 2024-02-06 16:50_

> Shouldn't it only count if a method is called on the string itself?

You're right, this rule only applies to the string itself, plus any parent expressions that the string is in. The example you gave is definitely something we should catch. Luckily, I think I have a good idea of why this is happening, and I'll try to make a fix for this soon.

A parent expression of the literal would be something like `func("{name}")` - something like `logging.info("{var}")` shouldn't be excluded by this rule, because we aren't calling a method on `info("{var}")` - rather, it _is_ the method that is being called.

---

_Assigned to @snowsignal by @snowsignal on 2024-02-06 20:56_

---

_Label `question` removed by @charliermarsh on 2024-02-07 22:42_

---

_Label `bug` added by @charliermarsh on 2024-02-07 22:42_

---

_Closed by @snowsignal on 2024-02-08 15:00_

---
