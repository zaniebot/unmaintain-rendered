---
number: 13339
title: "Formatter: Different behavior of `line-length` between ruff and black for docstrings and comments"
type: issue
state: closed
author: kevalmorabia97
labels:
  - question
assignees: []
created_at: 2024-09-12T11:20:57Z
updated_at: 2024-09-12T12:01:36Z
url: https://github.com/astral-sh/ruff/issues/13339
synced_at: 2026-01-07T13:12:15-06:00
---

# Formatter: Different behavior of `line-length` between ruff and black for docstrings and comments

---

_Issue opened by @kevalmorabia97 on 2024-09-12 11:20_

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


I am using `ruff==0.6.4`. I have been using Ruff linter for quite some time alongside black formatter. Now I would like to use Ruff Formatter instead of black but my issue is with the handling of `line-length` between Ruff and Black. With Black, line length is only enforced for code blocks and not for docstrings or comments. But Ruff's `tool.ruff.line-length` is a universal length restriction and causes `E501 Line too long` for hundreds of lines making it extremely difficult to manually fix them all. Plus I would prefer to leave those as is as they are just comments or docstrings and I don't want the linter to complain for them. 

Is there a way to configure length restriction to ignore docstrings and comments? Or perhaps allow a different length for linter and formatter?

---

_Label `question` added by @MichaReiser on 2024-09-12 11:32_

---

_Comment by @MichaReiser on 2024-09-12 11:35_

Hy @kevalmorabia97 

My recommendation is to disable `E501` and rather formatting related lint runes when using `ruff format`by adding them to `ignore = ["E501"]`. Annoter common setup is to keep `E501` enabled and increase the `[max-line-length`](https://docs.astral.sh/ruff/settings/#lint_pycodestyle_max-line-length) and [`max-doc-length`](https://docs.astral.sh/ruff/settings/#lint_pycodestyle_max-doc-length) settings to find very long lines that the formatter wasn't able to break lines into shorter lines automatically. 

---

_Comment by @kevalmorabia97 on 2024-09-12 12:01_

Thanks @MichaReiser `[max-line-length]` was exactly what I needed!

---

_Closed by @kevalmorabia97 on 2024-09-12 12:01_

---
