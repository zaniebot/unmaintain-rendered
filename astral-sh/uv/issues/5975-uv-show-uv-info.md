---
number: 5975
title: "`uv show` / `uv info`"
type: issue
state: open
author: nikhilweee
labels:
  - enhancement
  - cli
assignees: []
created_at: 2024-08-09T19:56:50Z
updated_at: 2025-08-16T21:55:51Z
url: https://github.com/astral-sh/uv/issues/5975
synced_at: 2026-01-10T01:23:54Z
---

# `uv show` / `uv info`

---

_Issue opened by @nikhilweee on 2024-08-09 19:56_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Now that uv has support for managing projects, it would be great to know the current _state_ of uv. By state, I mean:
* If a project is detected, what's the name of the project?
* What is the path to the python interpreter that will be used
* What virtual environment will be used
* ... and so on.


For reference, pdm has `pdm info` which shows information about the current state of the environment. Similarly, rye has `rye show` which does something similar. It would be nice if `uv` supported a similar command.

Not sure if this is already a point of discussoin in #5966

---

_Label `enhancement` added by @zanieb on 2024-08-09 22:15_

---

_Label `cli` added by @zanieb on 2024-08-09 22:15_

---

_Comment by @nikhilweee on 2024-09-04 20:27_

I think if there's a way to know the current venv being used that itself would be a great addition.

---

_Referenced in [astral-sh/uv#8755](../../astral-sh/uv/issues/8755.md) on 2024-11-01 16:30_

---

_Referenced in [astral-sh/uv#10666](../../astral-sh/uv/issues/10666.md) on 2025-01-31 14:34_

---

_Comment by @Flimm on 2025-08-16 21:55_

I would expect the command `uv show` to take an additional argument. I feel like saying "uv show what?". The verb "show" in English takes an object. 

The command `pip show <packagename>` takes a package name as an argument, and `uv pip show <packagename>` follows this design. The `apt` command also takes a package name as an argument: `apt show <packagename>` (on Debian and Debian-based Linux distributions). NPM also follows this pattern: `npm show <packagename>`

I'm not against the idea of this subcommand in principle, I just would like it to be named something other than "show". I think `rye show` is the only example I can find of a show subcommand that does not take an additional argument.

---
