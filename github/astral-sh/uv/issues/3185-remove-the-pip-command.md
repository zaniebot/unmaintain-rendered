---
number: 3185
title: Remove the pip command
type: issue
state: closed
author: inoa-jboliveira
labels:
  - duplicate
assignees: []
created_at: 2024-04-22T13:36:02Z
updated_at: 2024-04-22T14:54:08Z
url: https://github.com/astral-sh/uv/issues/3185
synced_at: 2026-01-07T13:12:17-06:00
---

# Remove the pip command

---

_Issue opened by @inoa-jboliveira on 2024-04-22 13:36_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Suggestion to remove the "pip" command from uv and just move all its subcommands to top level (install, freeze, list, sync, compile...). Instead of `uv pip install foo` we would be writing `uv install foo`.

### Pros

- Makes it easier to select executable uv/pip when writing scripts that could support both options in a transition period
- Pip is the name of another project and the interface is not identical
- Commands become shorter and direct to the point.
- In a couple years no one will even remember what pip is.
- It is still early in the uv life so that such a big change is still possible.

### Cons:

- Familiarity with the pip name, maybe
- uv plans to have several other tools (like `uv run`), so a command like `uv check` could not be obvious that it is related to installed packages. 



---

_Comment by @zanieb on 2024-04-22 14:23_

Hi! Please see #1899.

`uv install` will not be the same interface of `uv pip install` — we don't want to support all of the complexity of pip in our new workflow.

---

_Closed by @zanieb on 2024-04-22 14:23_

---

_Label `duplicate` added by @zanieb on 2024-04-22 14:23_

---

_Comment by @inoa-jboliveira on 2024-04-22 14:52_

That is good news. Thank you

---

_Comment by @zanieb on 2024-04-22 14:54_

You're welcome! Thanks for sharing your feedback.

---
