```yaml
number: 12658
title: Error in output in single line
type: issue
state: closed
author: pwsandoval
labels:
  - question
assignees: []
created_at: 2024-08-04T05:13:50Z
updated_at: 2025-08-19T13:27:32Z
url: https://github.com/astral-sh/ruff/issues/12658
synced_at: 2026-01-12T15:54:52Z
```

# Error in output in single line

---

_@pwsandoval_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

It's a very quick thing and maybe solved with a flag that already exists but you didn't find it. Today I updated ruff to the latest version and I see that now the check command is more verbose and gives more details than previous versions:

Now:

![ruff_now](https://github.com/user-attachments/assets/8e6147e2-32bc-48a5-a4ae-f8ca1ad9bd4f)

Before (from the internet) because I didn't take a screen before:

<img width="910" alt="ruff_before" src="https://github.com/user-attachments/assets/05550db9-2e30-4419-bf48-0f49abe1e40c">

Basically it prints one line for each error, instead of 4 lines spelling out the position with ^^^^^.

---

_Comment by @ember91 on 2024-08-04 07:27_

This is intended and was introduced with ruff 0.5.0. See https://astral.sh/blog/ruff-v0.5.0

Pass `-output-format=concise` for the previous behavior.

---

_Label `question` added by @MichaReiser on 2024-08-04 07:42_

---

_Closed by @charliermarsh on 2024-08-05 02:13_

---

_Comment by @mmarras on 2025-08-19 13:23_

Is there an option to set the output-fomat flag in the pyproject.toml too? 

---

_Comment by @mmarras on 2025-08-19 13:27_

> Is there an option to set the output-fomat flag in the pyproject.toml too?

Found it:

```toml
[tool.ruff]
output-format = 'concise'
```

---
