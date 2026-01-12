```yaml
number: 19623
title: subprocess modernize rules
type: issue
state: open
author: kaddkaka
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-07-29T21:02:06Z
updated_at: 2025-07-30T06:15:32Z
url: https://github.com/astral-sh/ruff/issues/19623
synced_at: 2026-01-12T15:54:56Z
```

# subprocess modernize rules

---

_@kaddkaka_

### Summary

subprocess package has a "new" and "old" API, see the in-source docstring below.

I think it would be valuable to have lint rules that warn when using the old API and recommend using the new one instead.

```py
Main API
========
run(...): Runs a command, waits for it to complete, then returns a
          CompletedProcess instance.
Popen(...): A class for flexibly executing a command in a new process

Constants
---------
DEVNULL: Special value that indicates that os.devnull should be used
PIPE:    Special value that indicates a pipe should be created
STDOUT:  Special value that indicates that stderr should go to stdout


Older API
=========
call(...): Runs a command, waits for it to complete, then returns
    the return code.
check_call(...): Same as call() but raises CalledProcessError()
    if return code is not 0
check_output(...): Same as check_call() but returns the contents of
    stdout instead of a return code
getoutput(...): Runs a command in the shell, waits for it to complete,
    then returns the output
getstatusoutput(...): Runs a command in the shell, waits for it to complete,
    then returns a (exitcode, output) tuple
```

---

_Label `rule` added by @ntBre on 2025-07-29 22:18_

---

_Label `needs-decision` added by @ntBre on 2025-07-29 22:18_

---

_Comment by @MichaReiser on 2025-07-30 06:15_

Reference: https://docs.python.org/3/library/subprocess.html#older-high-level-api

---
