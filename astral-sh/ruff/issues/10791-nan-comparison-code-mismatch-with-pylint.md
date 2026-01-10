```yaml
number: 10791
title: nan-comparison code mismatch with Pylint
type: issue
state: closed
author: BlindChickens
labels:
  - bug
  - rule
assignees: []
created_at: 2024-04-05T16:56:23Z
updated_at: 2024-04-22T20:41:23Z
url: https://github.com/astral-sh/ruff/issues/10791
synced_at: 2026-01-10T11:09:53Z
```

# nan-comparison code mismatch with Pylint

---

_Issue opened by @BlindChickens on 2024-04-05 16:56_

In the (W)arning category of Pylint `nan-comparison` has code `W0177`. [Here](https://pylint.readthedocs.io/en/latest/user_guide/checkers/features.html#:~:text=nan%2Dcomparison%20(W0177)%3A)

In Ruff it is coded to `W0117`.

Is it a mistake?

Thanks in advance
<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "W0117", "nan-comparison"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Label `bug` added by @charliermarsh on 2024-04-07 03:34_

---

_Label `rule` added by @charliermarsh on 2024-04-07 03:34_

---

_Comment by @charliermarsh on 2024-04-07 03:35_

Ahh thank you! We'll correct it. Looks like an oversight.

---

_Comment by @WindowGenerator on 2024-04-07 11:35_

@charliermarsh Hi!
Can I fix it?

---

_Comment by @charliermarsh on 2024-04-07 16:00_

Feel free, thanks! You'll need to update the code in `codes.rs`, and then also add a redirect from the old to new code in `rule_redirects.rs`.

---

_Comment by @charliermarsh on 2024-04-12 02:12_

I'm gonna go ahead and fix this real quick.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-12 02:14_

---

_Closed by @charliermarsh on 2024-04-12 02:49_

---

_Comment by @BlindChickens on 2024-04-22 20:41_

@charliermarsh 
Sorry to necropost like this. But I forgot to ask that this issue be updated to reflect this fix.
[here](https://github.com/astral-sh/ruff/issues/970#:~:text=nan%2Dcomparison%20/%20W0177%20(PLW0117))

---
