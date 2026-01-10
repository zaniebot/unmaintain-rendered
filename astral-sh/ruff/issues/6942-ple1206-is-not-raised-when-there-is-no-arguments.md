---
number: 6942
title: PLE1206 is not raised when there is no arguments passed to the logging method call.
type: issue
state: closed
author: pavel-beaufort
labels:
  - bug
assignees: []
created_at: 2023-08-28T12:49:37Z
updated_at: 2023-08-28T17:55:16Z
url: https://github.com/astral-sh/ruff/issues/6942
synced_at: 2026-01-10T01:22:46Z
---

# PLE1206 is not raised when there is no arguments passed to the logging method call.

---

_Issue opened by @pavel-beaufort on 2023-08-28 12:49_

`PLE1206` (logging-too-few-args) error is not shown for such code snippet:
```
logger.error("%r: no filename.")
```
0.0.286


---

_Label `bug` added by @charliermarsh on 2023-08-28 14:45_

---

_Comment by @charliermarsh on 2023-08-28 17:25_

Pylint doesn't raise on single-argument either. I'm not sure why.

---

_Comment by @charliermarsh on 2023-08-28 17:55_

Okay, so I think this is intentional. If no message arguments are provided, the `logging` module treats the string as a literal. So this does not error:

```python
logging.error("%s %s")
```

(It logs the literal `%s %s`.)

However, this _does_ error:

```python
logging.error("%s %s", 1)
```

With:

```text
--- Logging error ---
Traceback (most recent call last):
  File "/Users/crmarsh/.local/share/rtx/installs/python/3.11.2/lib/python3.11/logging/__init__.py", line 1110, in emit
    msg = self.format(record)
          ^^^^^^^^^^^^^^^^^^^
  File "/Users/crmarsh/.local/share/rtx/installs/python/3.11.2/lib/python3.11/logging/__init__.py", line 953, in format
    return fmt.format(record)
           ^^^^^^^^^^^^^^^^^^
  File "/Users/crmarsh/.local/share/rtx/installs/python/3.11.2/lib/python3.11/logging/__init__.py", line 687, in format
    record.message = record.getMessage()
                     ^^^^^^^^^^^^^^^^^^^
  File "/Users/crmarsh/.local/share/rtx/installs/python/3.11.2/lib/python3.11/logging/__init__.py", line 377, in getMessage
    msg = msg % self.args
          ~~~~^~~~~~~~~~~
TypeError: not enough arguments for format string
Call stack:
  File "/Users/crmarsh/workspace/ruff/foo.py", line 3, in <module>
    logging.error("%s %s", 1)
Message: '%s %s'
Arguments: (1,)
```

So, while it's a strange behavior, IMO it would be incorrect to raise an error here since omitting message arguments is a valid usage.

---

_Closed by @charliermarsh on 2023-08-28 17:55_

---
