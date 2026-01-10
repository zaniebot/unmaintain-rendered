---
number: 1834
title: "Consider to hide `--index-url` and `--extra-index-url` if `--emit-index-url` flag is not presented"
type: issue
state: closed
author: drjackild
labels: []
assignees: []
created_at: 2024-02-21T21:39:02Z
updated_at: 2024-02-23T09:25:26Z
url: https://github.com/astral-sh/uv/issues/1834
synced_at: 2026-01-10T01:23:09Z
---

# Consider to hide `--index-url` and `--extra-index-url` if `--emit-index-url` flag is not presented

---

_Issue opened by @drjackild on 2024-02-21 21:39_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Hey guys! As the title said, I wonder if the `uv` could hide `--index-url` and `--extra-index-url` if the `--emit-index-url` flag is not presented. Otherwise, if you have a private package index, this data could be accidentally committed to the repository. `pip-tools` hides the index and extra index both from the command in the comment and of course does not include them in the body of the compiled files.

I've created a small PR to do this, but if this is "by design", than I'll just close it :)

---

_Comment by @charliermarsh on 2024-02-21 21:39_

We hide these by default in the body, right? So would this mainly be to remove them from the comment header?

---

_Referenced in [astral-sh/uv#1835](../../astral-sh/uv/pulls/1835.md) on 2024-02-21 21:40_

---

_Comment by @drjackild on 2024-02-21 21:54_

> We hide these by default in the body, right? So would this mainly be to remove them from the comment header?

Yes, exactly, only in the header comment, the body of the requirements is looking good

---

_Closed by @drjackild on 2024-02-23 09:25_

---
