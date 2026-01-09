---
number: 8009
title: B005 doc error
type: issue
state: closed
author: ahmedabdou14
labels:
  - documentation
assignees: []
created_at: 2023-10-17T11:49:07Z
updated_at: 2023-10-17T22:32:58Z
url: https://github.com/astral-sh/ruff/issues/8009
synced_at: 2026-01-07T13:12:15-06:00
---

# B005 doc error

---

_Issue opened by @ahmedabdou14 on 2023-10-17 11:49_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

## Hi, was looking at rule B005 and found an error in the example provided
### [strip-with-multi-characters (B005)](https://docs.astral.sh/ruff/rules/strip-with-multi-characters/)
![image](https://github.com/astral-sh/ruff/assets/104530599/d5193519-32a3-4e59-a66b-37d750c1d92b)

```python
# Wrong output (used in docs) 
"text.txt".strip(".txt")  # "ex" 

# Correct output
"text.txt".strip(".txt")  # "e"
```

Was planning to open a PR for it but couldn't find the docs project, would appreciate if you elaborate on how to contribute to the docs ^_^

---

_Comment by @T-256 on 2023-10-17 13:17_

@Ahmed-Abdou14 Rules documents are specified in rules sources via doc-comments, For this case:
https://github.com/astral-sh/ruff/blob/dc6b4ad2b4b8d89757bb74968d1865b3e11c9b7a/crates/ruff_linter/src/rules/flake8_bugbear/rules/strip_with_multi_characters.rs#L35-L38

---

_Comment by @charliermarsh on 2023-10-17 13:34_

Oh thanks, PR welcome! You can edit the Rust file directly.

---

_Label `documentation` added by @charliermarsh on 2023-10-17 13:34_

---

_Comment by @ahmedabdou14 on 2023-10-17 21:54_

> Oh thanks, PR welcome! You can edit the Rust file directly.

Thank you @T-256 & @charliermarsh 
I have created a [PR](https://github.com/astral-sh/ruff/pull/8028)
A tiny contribution but many to come! Thank you for the amazing tool ^_^

---

_Comment by @charliermarsh on 2023-10-17 22:32_

Thank you @Ahmed-Abdou14, much appreciated! (Closed by https://github.com/astral-sh/ruff/pull/8028.)

---

_Closed by @charliermarsh on 2023-10-17 22:32_

---
