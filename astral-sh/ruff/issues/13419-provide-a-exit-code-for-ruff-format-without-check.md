```yaml
number: 13419
title: "provide a exit code for `ruff format` (without --check) for source file syntax error"
type: issue
state: closed
author: trim21
labels:
  - question
  - cli
assignees: []
created_at: 2024-09-20T05:54:51Z
updated_at: 2024-10-27T19:01:19Z
url: https://github.com/astral-sh/ruff/issues/13419
synced_at: 2026-01-12T15:54:53Z
```

# provide a exit code for `ruff format` (without --check) for source file syntax error

---

_@trim21_

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

`ruff format /path/to/file` now have 2 exit code, 0 for success and 2 for errors.

> 0 if Ruff terminates successfully, regardless of whether any files were formatted.
> 2 if Ruff terminates abnormally due to invalid configuration, invalid CLI options, or an internal error.

I proposal to add a exit code 1 for source file syntax error.

This would be helpful for IDE/editor plugins to know it's expected error or unexpected internal error.



---

_Label `question` added by @MichaReiser on 2024-09-20 08:10_

---

_Label `cli` added by @MichaReiser on 2024-09-20 08:10_

---

_Comment by @MichaReiser on 2024-09-20 08:10_

Can you tell me more about your use case? Our long-term goal is that editors use the LSP to integrate Ruff because of performance and correctness concerns once we start adding multifile analysis support:

* Ruff needs to consider unsaved changes to avoid false positives. 
* Restarting Ruff on every keystroke would require re-checking your entire project and not just a single file. Yes, we plan to add some form of caching but the very least that Ruff has to do is to stat every referenced file to see if it has changed.



---

_Closed by @trim21 on 2024-10-27 19:01_

---
