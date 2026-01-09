---
number: 9114
title: "Add rule to flag comments with #:"
type: issue
state: open
author: Stigjb
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-12-13T13:32:20Z
updated_at: 2023-12-13T15:48:47Z
url: https://github.com/astral-sh/ruff/issues/9114
synced_at: 2026-01-07T13:12:15-06:00
---

# Add rule to flag comments with #:

---

_Issue opened by @Stigjb on 2023-12-13 13:32_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

The E262/E265 rules checks if one space is used after inline/block comments. However, to be consistent with Pycodestyle, they will accept both `# ` and `#: ` as valid inline comments, which is inconsistent with the wording in PEP8. I believe I've heard that this quirk is because of some conventional usage of comments for documenting class attributes, but I'm unfortunately unable to find a reference.

I would like a linting rule that follows PEP 8 to the letter and allows only `# ` in block and/or inline comments.

Example:

```python
#bad block comment
#:bad block comment
#: bad block comment (but allowed by E265)
# good block comment

a = 1  #bad inline comment
b = 1  #:bad inline comment
c = 1  #: bad inline comment (but allowed by E262)
d = 1  # good inline comment
```

EDIT

I found a Stack Overflow question about `#:`: https://stackoverflow.com/a/37352230. It looks like the source of the convention might be the Sphinx documentation generator

---

_Label `rule` added by @charliermarsh on 2023-12-13 15:48_

---

_Label `needs-decision` added by @charliermarsh on 2023-12-13 15:48_

---
