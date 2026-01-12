```yaml
number: 9131
title: "`TCH` rules sometimes don't autofix"
type: issue
state: closed
author: tylerlaprade
labels:
  - bug
assignees: []
created_at: 2023-12-14T16:28:35Z
updated_at: 2023-12-14T18:15:50Z
url: https://github.com/astral-sh/ruff/issues/9131
synced_at: 2026-01-12T15:54:48Z
```

# `TCH` rules sometimes don't autofix

---

_@tylerlaprade_

I haven't figured out the pattern yet, but there are some edge cases where the CLI says the fixes are autofixable, but doesn't do so even with the `--fix` flag and the appropriate config:
<img width="1167" alt="image" src="https://github.com/astral-sh/ruff/assets/5475199/a8a0942a-594a-4afa-b9d7-3e616ab8b0f1">
<img width="384" alt="image" src="https://github.com/astral-sh/ruff/assets/5475199/607fd022-05ea-472c-aba6-a9ecdfaa8cad">


---

_Comment by @charliermarsh on 2023-12-14 16:41_

If you're able to share even a single file or snippet that exhibits this behavior, I'd love to investigate!

---

_Label `bug` added by @charliermarsh on 2023-12-14 16:41_

---

_Comment by @tylerlaprade on 2023-12-14 17:34_

@charliermarsh, I'll DM you on Discord 

---

_Comment by @tylerlaprade on 2023-12-14 17:49_

Sorry, I didn't notice this stack trace earlier:
```
error: Panicked while linting abacus/permissions/default.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at /Users/runner/work/ruff/ruff/crates/ruff_text_size/src/range.rs:48:9:
assertion failed: start.raw <= end.raw
Backtrace:    0: _rust_eh_personality
   1: _main
   2: _rust_eh_personality
   3: _rust_eh_personality
   4: _rust_eh_personality
   5: _rust_eh_personality
   6: __rjem_je_witnesses_cleanup
   7: __rjem_je_witnesses_cleanup
   8: _main
   9: _main
  10: _main
  11: _main
  12: _main
  13: _main
  14: _main
  15: _main
  16: _main
  17: _main
  18: __rjem_je_witnesses_cleanup
  19: _main
  20: _main
  21: _main
  22: __rjem_je_witnesses_cleanup
  23: _main
  24: _main
  25: _rust_eh_personality
  26: __pthread_joiner_wake
  ```

---

_Comment by @tylerlaprade on 2023-12-14 17:55_

Here's one of the errors causing the above stack trace:
```
from django.contrib.auth.models import AbstractBaseUser, AnonymousUser
...
def foo(
    self, user: AbstractBaseUser | AnonymousUser, view: 'type[CondorBaseViewSet]'
)
```
When I run `ruff check`, I simply see 3 errors and a message saying all 3 are fixable. When I run `ruff check --fix`, I get the stack trace and no errors.

---

_Comment by @tylerlaprade on 2023-12-14 17:56_

Manually quoting `'AbstractBaseUser | AnonymousUser'` and then saving the file (to trigger autofix) resolved all errors correctly.

---

_Comment by @charliermarsh on 2023-12-14 18:01_

Thanks! I'm gonna file a few issues here based on the info you provided.

---

_Comment by @charliermarsh on 2023-12-14 18:15_

Filing as:

- https://github.com/astral-sh/ruff/issues/9135
- https://github.com/astral-sh/ruff/issues/9136
- https://github.com/astral-sh/ruff/issues/9137

---

_Closed by @charliermarsh on 2023-12-14 18:15_

---
