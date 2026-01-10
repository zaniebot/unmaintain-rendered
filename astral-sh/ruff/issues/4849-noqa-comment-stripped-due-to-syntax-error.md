```yaml
number: 4849
title: "`noqa` comment stripped due to syntax error elsewhere in file"
type: issue
state: closed
author: stinodego
labels:
  - bug
  - suppression
assignees: []
created_at: 2023-06-04T14:32:56Z
updated_at: 2023-06-05T17:32:08Z
url: https://github.com/astral-sh/ruff/issues/4849
synced_at: 2026-01-10T11:09:47Z
```

# `noqa` comment stripped due to syntax error elsewhere in file

---

_Issue opened by @stinodego on 2023-06-04 14:32_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Ruff indicates an unused noqa statement, when in fact it's fine. There is a syntax error in the function definition that triggers this, if the `*` is removed, everything is fine.

```python
def function(*):
    """Veeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeery long."""  # noqa: W505
```

This is on ruff version `0.0.270`.

---

_Comment by @charliermarsh on 2023-06-04 15:04_

Thanks, I actually thought we added handling for this case at some point. Will revisit!

---

_Comment by @stinodego on 2023-06-04 15:07_

> Thanks, I actually thought we added handling for this case at some point. Will revisit!

I know, I have reported something similar before:
https://github.com/charliermarsh/ruff/issues/2853

Not sure what caused this one to resurface.

---

_Comment by @charliermarsh on 2023-06-05 02:38_

(I believe this regressed when we started generating fixes on every pass, regardless of whether they're applied.)

---

_Comment by @charliermarsh on 2023-06-05 02:44_

@MichaReiser - Any opinion on the right behavior here? The issue is that if we hit a syntax error, we fail to identify the W505 violation in the example above, and so the `noqa` is considered unused and removed.

We could...

1. Avoid autofixing at all if we hit a syntax error.
2. Avoid autofixing RUF100 violations (unused suppression comments)  if we hit a syntax error.
3. Avoid _raising_ RUF100 violations if we hit a syntax error.

---

_Label `bug` added by @charliermarsh on 2023-06-05 02:44_

---

_Label `noqa` added by @charliermarsh on 2023-06-05 02:44_

---

_Comment by @MichaReiser on 2023-06-05 06:10_

@charliermarsh My preference is 3. The way I think about this is that rules should only run if their input is correct. What the input is, differs from rule to rule:

* Physical lines: always valid
* logical lines: valid tokens
* AST rules: parsable source tree
* RUF100: All configured rules ran

That's why I think RUF100 should not run if any of the other phases returned an `Err`. 

---

_Comment by @stinodego on 2023-06-05 06:12_

For what it's worth, I also believe 3 is the way to go. I don't like all the linting violations popping up in seemingly unrelated places while I'm writing some code.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-05 12:59_

---

_Comment by @charliermarsh on 2023-06-05 16:38_

Sounds good, will fix today.

---

_Closed by @charliermarsh on 2023-06-05 17:32_

---
