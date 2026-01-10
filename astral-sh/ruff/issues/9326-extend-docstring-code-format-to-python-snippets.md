---
number: 9326
title: Extend docstring-code-format to python snippets in MarkDown, RST etc
type: issue
state: closed
author: peterjc
labels:
  - docstring
  - formatter
assignees: []
created_at: 2023-12-30T22:03:54Z
updated_at: 2024-01-02T19:52:10Z
url: https://github.com/astral-sh/ruff/issues/9326
synced_at: 2026-01-10T01:22:49Z
---

# Extend docstring-code-format to python snippets in MarkDown, RST etc

---

_Issue opened by @peterjc on 2023-12-30 22:03_

With [ruff](https://astral.sh/blog/ruff-v0.1.8) introducing the ``docstring-code-format`` setting, the tool might start to follow what https://pypi.org/project/blacken-docs/ offers - not just black style formatting to doctests within Python files, but also doctests and Python snippets within reStructuredText files (``*.rst``, often used for Python project documentation), Markdown files (``*.md``), and even LaTeX files (``*.tex``).

Is this something you think could be on the ruff roadmap? Or might it be better to look at other ways to achieve this, e.g. one might try modifying ``blacken-docs`` to invoke either ``black`` (as now) or the ``ruff`` formatter internally.

---

_Label `docstring` added by @charliermarsh on 2023-12-31 12:40_

---

_Label `formatter` added by @charliermarsh on 2023-12-31 12:40_

---

_Comment by @charliermarsh on 2023-12-31 12:43_

👍 We do plan to support this though not sure if it's still captured in any open issues. @BurntSushi -- do you know if there's an issue for this?

---

_Comment by @charliermarsh on 2024-01-02 18:45_

Ahh found it, I believe this is the same as https://github.com/astral-sh/ruff/issues/8237.

---

_Comment by @peterjc on 2024-01-02 19:52_

Agreed, closing in favour of #8237. I'm surprised I missed that in my issue searches. Thanks!

---

_Closed by @peterjc on 2024-01-02 19:52_

---
