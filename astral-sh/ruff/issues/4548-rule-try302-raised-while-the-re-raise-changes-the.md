```yaml
number: 4548
title: Rule TRY302 raised while the re-raise changes the error
type: issue
state: closed
author: 153957
labels:
  - bug
assignees: []
created_at: 2023-05-20T17:23:38Z
updated_at: 2023-05-21T15:49:54Z
url: https://github.com/astral-sh/ruff/issues/4548
synced_at: 2026-01-10T11:09:47Z
```

# Rule TRY302 raised while the re-raise changes the error

---

_Issue opened by @153957 on 2023-05-20 17:23_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

With code like the example below I got this unexpected error:
> TRY302 Remove exception handler; error is immediately re-raised

Although the error is immediately re-raised, it is modified, the causes are removed.

Code to reproduce:
```python
try:
    self.assertEqual(cells, interlinked)
except AssertionError as error:
    # Catch and raise again without causes
    raise error from None
```

Enabled ALL settings.
Version: ruff 0.0.269

---

_Comment by @JonathanPlasse on 2023-05-20 17:33_

Good catch!

---

_Label `bug` added by @charliermarsh on 2023-05-20 18:18_

---

_Comment by @JonathanPlasse on 2023-05-21 09:52_

Would you like to work on it?

---

_Comment by @153957 on 2023-05-21 11:21_

I'd like to, but have no experience writing Rust, so probably can't without some pointers.

I assume fixing this requires changes to the logic here: https://github.com/charliermarsh/ruff/blob/fc63c6f2e260760963c5fdea2d9d4e0cf38ec58a/crates/ruff/src/rules/tryceratops/rules/useless_try_except.rs#L52-L55
Somehow checking it there is a `from` after the raise statement, i.e. following the expression for which the name is checked, if that is the case just `return None;`.

And add a new test case(s) here: https://github.com/charliermarsh/ruff/blob/fc63c6f2e260760963c5fdea2d9d4e0cf38ec58a/crates/ruff/resources/test/fixtures/tryceratops/TRY302.py#L4

---

_Comment by @JonathanPlasse on 2023-05-21 11:40_

You can look up `cause` in `ast::StmtRaise`.
It should be only changing the pattern matching.

---

_Comment by @153957 on 2023-05-21 13:07_

Thank you for the hint, PR is up.

---

_Closed by @charliermarsh on 2023-05-21 15:49_

---
