---
number: 7782
title: "Formatter: include filenames in ruff format --check"
type: issue
state: closed
author: matt-gardner
labels:
  - cli
  - formatter
assignees: []
created_at: 2023-10-03T14:24:57Z
updated_at: 2024-04-11T22:52:07Z
url: https://github.com/astral-sh/ruff/issues/7782
synced_at: 2026-01-07T13:12:15-06:00
---

# Formatter: include filenames in ruff format --check

---

_Issue opened by @matt-gardner on 2023-10-03 14:24_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Apologies if this is a duplicate or already fixed; the closest issue I found to this one was #7231.  This issue is similar, but much smaller scoped.  When I run `black --check`, it gives me a list of filenames that would be reformatted, not just a count.  When I run `ruff format --check`, it only gives me statistics.  The list of filenames is much more useful when the format check is run during CI, and it'd be nice to add it to ruff.

I'm running ruff 0.0.291.  Here's a sample output from black and from ruff:

```bash
~/my_code % ruff format --check .
warning: `ruff format` is a work-in-progress, subject to change at any time, and intended only for experimentation.
5 files would be reformatted, 264 files left unchanged
```

```bash
~/my_code % black --check .         
would reformat /path/to/my_code/my_file.py

Oh no! 💥 💔 💥
1 file would be reformatted, 268 files would be left unchanged.
```

---

_Label `cli` added by @charliermarsh on 2023-10-03 14:27_

---

_Label `formatter` added by @charliermarsh on 2023-10-03 14:27_

---

_Comment by @charliermarsh on 2023-10-03 14:27_

Good call, we should fix this!

---

_Added to milestone `Formatter: Beta` by @charliermarsh on 2023-10-03 14:27_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-03 16:35_

---

_Referenced in [astral-sh/ruff#7788](../../astral-sh/ruff/pulls/7788.md) on 2023-10-03 18:19_

---

_Closed by @charliermarsh on 2023-10-03 18:50_

---

_Comment by @matt-gardner on 2023-10-03 19:00_

Awesome, thanks!

---

_Comment by @charliermarsh on 2023-10-03 19:04_

No prob, thanks for reporting.

---

_Comment by @AdrianB-sovo on 2024-04-11 22:52_

@charliermarsh It would also be useful to have that when running it **without** `--check` (or at least allow to enable it with an option).
**Usecase:** I use the pre-commit config to run Ruff locally before each commit, but also on the CI (for all files).
- Locally, I want to automatically change the files with the formatter on a commit.
- In the CI, I want to see what files are not correctly formatted.

And thanks for the amazing tools (`Ruff` and `uv`) @charliermarsh! 😁

---
