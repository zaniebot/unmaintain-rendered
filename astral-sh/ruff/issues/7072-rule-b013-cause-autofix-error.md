```yaml
number: 7072
title: Rule B013 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-03T08:42:20Z
updated_at: 2023-09-03T13:31:15Z
url: https://github.com/astral-sh/ruff/issues/7072
synced_at: 2026-01-12T15:54:46Z
```

# Rule B013 cause autofix error

---

_@qarmin_

Ruff 0.0.287 (latest changes from main branch)

```
ruff --fix *.py  --no-cache --select B013
```

file content:
```
class _RetryWrapper(object):
      try:
          raise ndb.Return(result)
      except runtime.DeadlineExceededError:
        logging.debug(
            time.time() - start_time)
      except (*self.retriable_exceptions,) as exc:
        logging.debug('Tasklet is %r', tasklet)
```

error:
```
error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `3527004.py`, the rule codes B013, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

3527004.py:7:14: B013 A length-one tuple literal is redundant. Write `except *self.retriable_exceptions` instead of `except (*self.retriable_exceptions,)`.
Found 1 error.
```


[3527004.py.zip](https://github.com/astral-sh/ruff/files/12505526/3527004.py.zip)


---

_Label `bug` added by @charliermarsh on 2023-09-03 12:03_

---

_Label `fuzzer` added by @charliermarsh on 2023-09-03 12:03_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-03 13:15_

---

_Closed by @charliermarsh on 2023-09-03 13:31_

---
