```yaml
number: 2442
title: False positive from RUF100 with noqa comment to suppress SIM108
type: issue
state: closed
author: henribru
labels:
  - bug
assignees: []
created_at: 2023-02-01T15:24:51Z
updated_at: 2023-02-01T22:04:59Z
url: https://github.com/astral-sh/ruff/issues/2442
synced_at: 2026-01-12T15:54:42Z
```

# False positive from RUF100 with noqa comment to suppress SIM108

---

_@henribru_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->

RUF100 claims this noqa comment is unused, but if you remove it SIM108 will trigger:
https://ruff.pages.dev/#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-AfJsklAFHaGgdJs1Fl0SWAKtKkQGpAyCRy-Q7QPIyXQABWIl5INDyytG4qA4JczqCbnwYm6ZpK9D0hqEA6pIhav2WAAZYU6BgAAMRgHSLeaAdBNi7H2AsLQYAAC8YBfggBKMUGgNr7VgGFUAA

---

_Label `bug` added by @charliermarsh on 2023-02-01 15:27_

---

_Comment by @charliermarsh on 2023-02-01 15:29_

Hah... this makes sense, thank you for filing.

If the ternary contains comments, we avoid triggering `SIM108` -- this is helpful because comments are hard to preserve with ternaries anyway, e.g.:

```py
if a:
  # Some comment about why we do this.
  ...
else:
  # Some comment about why we do this.
  ...
```

So the presence of the `noqa` comment causes the violation _not_ to trigger, which makes it unused.


---

_Comment by @charliermarsh on 2023-02-01 15:30_

I have to think about the right fix.

We could:

- Always flag, and just not autofix when comments are present.
- Only skip if there are comments in the body (rather than at end-of-line).


---

_Comment by @henribru on 2023-02-01 15:34_

For now I'm working around it by adding RUF100 to the noqa comment (which is slightly meta in a way :D)

---

_Comment by @charliermarsh on 2023-02-01 15:41_

🤯 🤯 🤯 

You could also just add a comment to the body and remove the `noqa` altogether for now :)

---

_Comment by @charliermarsh on 2023-02-01 15:41_

But I'll try to fix this today. It affects more than this check, since we have this pattern in a few places.

---

_Closed by @charliermarsh on 2023-02-01 17:56_

---

_Comment by @henribru on 2023-02-01 22:04_

Thanks for the quick response as always!

---
