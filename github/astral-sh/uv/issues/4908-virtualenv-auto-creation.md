---
number: 4908
title: Virtualenv auto-creation
type: issue
state: closed
author: chrisrodrigue
labels:
  - question
assignees: []
created_at: 2024-07-08T20:31:17Z
updated_at: 2024-07-08T21:33:39Z
url: https://github.com/astral-sh/uv/issues/4908
synced_at: 2026-01-07T13:12:17-06:00
---

# Virtualenv auto-creation

---

_Issue opened by @chrisrodrigue on 2024-07-08 20:31_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Greetings,

Since `uv` conveniently works in place of `venv` and `virtualenv`, I just wanted to propose a really handy development feature that many users of `pdm` and `poetry` are enjoying. 

When invoking `pdm install` from the project root, the default behavior (unless configured otherwise) is to create a `.venv` folder in the root of the project which installs all the dependencies annotated in `pyproject.toml`. This behavior is [described here](https://pdm-project.org/latest/usage/venv/#virtualenv-auto-creation). I think `pdm install` implicitly performs `pdm init` and generates a lockfile if no existing lockfile is detected.

`poetry` takes a slightly different approach and defaults to installing the virtualenvs in a separate location, and requires the user to set `virtualenv.in-project` to true (although not in `pyproject.toml`, it is in another config file used by `poetry`).  This behavior is [described here](https://python-poetry.org/docs/basic-usage/#using-your-virtual-environment). Most users have `.venv` added to their `.gitignore` and in general I think it's nice to keep the `.venv` close to where it is being consumed.

 I think this would be a great and important capability if and when `uv` builds out a `uv install` command.

---

_Comment by @zanieb on 2024-07-08 20:46_

Hi!

We considered doing this for `uv pip install` in https://github.com/astral-sh/uv/pull/2665 but opted not after significant feedback from users. `uv sync` (which is our equivalent to the pdm or Poetry install commands) does this already.

---

_Label `question` added by @zanieb on 2024-07-08 20:47_

---

_Comment by @chrisrodrigue on 2024-07-08 20:48_

Ah okay, great! Thanks for the prompt reply!

---

_Comment by @chrisrodrigue on 2024-07-08 20:52_

I guess this is a semi-related question: will `pdm sync` work if the lockfile has been generated on another platform? I am not sure where `uv` stands right now when it comes to cross-platform lockfile generation/dependency resolution.

---

_Comment by @zanieb on 2024-07-08 20:53_

Yep! See #3347 

---

_Comment by @chrisrodrigue on 2024-07-08 21:00_

Incredible!! You guys are working on this at lightspeed. Eager to start using these features... might be worth updating the [README](https://github.com/astral-sh/uv#limitations) to highlight the capabilities? 

Also, is there any plan to install the current project as editable into the `.venv` by default (or a configuration option to do so) similar to `pdm`?

---

_Comment by @zanieb on 2024-07-08 21:07_

We'll update the README when we're ready :) we have documentation to write still

Yes, we do that too.

---

_Closed by @zanieb on 2024-07-08 21:33_

---
