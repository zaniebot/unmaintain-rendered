```yaml
number: 1853
title: Support linting for python files without extension
type: issue
state: closed
author: joshuachapnick-bc
labels: []
assignees: []
created_at: 2023-01-13T16:45:27Z
updated_at: 2023-01-13T17:13:07Z
url: https://github.com/astral-sh/ruff/issues/1853
synced_at: 2026-01-10T11:09:44Z
```

# Support linting for python files without extension

---

_Issue opened by @joshuachapnick-bc on 2023-01-13 16:45_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->

Is it possible to add support for linting files without `.py, .pyi` extensions (or override the check)? We use a CLI written in python that does not have a `.py` extension which causes ruff to fail when running.


---

_Comment by @charliermarsh on 2023-01-13 16:50_

If you pass the file directly (“ruff path/to/file/without/an/extension”), we should lint it regardless of the extension, which is what I’ve seen other projects do. Does that help here? Or insufficient?

---

_Comment by @joshuachapnick-bc on 2023-01-13 16:54_

That works locally but not on github actions for some reason. I have an action set up that runs `ruff <filename> --config pyproject.toml` and ruff returns `error <filename> is not supported; Ruff only supports .py and .pyi files`

---

_Comment by @joshuachapnick-bc on 2023-01-13 17:00_

Is there some flag I can pass to override that error?

---

_Comment by @joshuachapnick-bc on 2023-01-13 17:04_

Looks like my ruff version was out of date.

---

_Closed by @joshuachapnick-bc on 2023-01-13 17:04_

---

_Comment by @charliermarsh on 2023-01-13 17:11_

Ah ok, that was gonna be my next question. Great!

---

_Comment by @joshuachapnick-bc on 2023-01-13 17:13_

Thanks for the quick response!

---
