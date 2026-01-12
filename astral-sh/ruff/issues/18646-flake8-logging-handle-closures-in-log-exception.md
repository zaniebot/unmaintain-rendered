```yaml
number: 18646
title: "[`flake8-logging`] Handle closures in `log-exception-outside-except-handler` (`LOG004`)"
type: issue
state: open
author: dylwil3
labels:
  - bug
  - rule
  - preview
assignees: []
created_at: 2025-06-12T13:02:32Z
updated_at: 2025-09-02T21:09:14Z
url: https://github.com/astral-sh/ruff/issues/18646
synced_at: 2026-01-12T15:54:56Z
```

# [`flake8-logging`] Handle closures in `log-exception-outside-except-handler` (`LOG004`)

---

_@dylwil3_

I think this example might be a false positive:

https://github.com/zulip/zulip/blob/3dc54a10d7cbfb6f6f66a868a31d17ee9e215f53/zerver/worker/email_senders_base.py#L28-L45

It looks like a closure defined in the except handler will preserve its `exc_info` in the same call stack:

```pycon
>>> import logging
>>> l = []
>>> def foo(f): f()
...
>>> try:
...     raise ValueError("my error")
... except ValueError:
...     logging.exception("building on_failure")
...     def on_failure():
...         logging.exception("on_failure called")
...     foo(on_failure)
...     l.append(on_failure)
...
ERROR:root:building on_failure
Traceback (most recent call last):
  File "<python-input-19>", line 2, in <module>
    raise ValueError("my error")
ValueError: my error
ERROR:root:on_failure called
Traceback (most recent call last):
  File "<python-input-19>", line 2, in <module>
    raise ValueError("my error")
ValueError: my error
>>> l[-1]()
ERROR:root:on_failure called
NoneType: None
```

but not if you store it somewhere else and call it later.

_Originally posted by @ntBre in https://github.com/astral-sh/ruff/issues/18603#issuecomment-2957497956_
            

---

_Label `bug` added by @ntBre on 2025-09-02 21:09_

---

_Label `rule` added by @ntBre on 2025-09-02 21:09_

---

_Label `preview` added by @ntBre on 2025-09-02 21:09_

---
