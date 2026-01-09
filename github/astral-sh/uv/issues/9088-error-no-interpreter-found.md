---
number: 9088
title: "error: No interpreter found -- "
type: issue
state: open
author: EdmundsEcho
labels:
  - error messages
assignees: []
created_at: 2024-11-13T16:01:39Z
updated_at: 2025-04-16T03:52:34Z
url: https://github.com/astral-sh/uv/issues/9088
synced_at: 2026-01-07T13:12:18-06:00
---

# error: No interpreter found -- 

---

_Issue opened by @EdmundsEcho on 2024-11-13 16:01_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
I am new to uv.  

## The problem

When I tried to initialize a new project, I received the following: 

```sh
❯ uv init TestProject --verbose
DEBUG uv 0.5.1 (f399a5271 2024-11-08)
DEBUG Reading Python requests from version file at `/Users/edmund/Downloads/.python-version`
DEBUG Searching for Python interpreter with executable name `3.10-dev`
DEBUG Checking for Python interpreter at executable name `3.10-dev`
error: No interpreter found for executable name `3.10-dev` in virtual environments, managed installations, or search path
```

## The fix

Removed the `.python-version` present in the root folder from where I was trying to create the new project.


## Longer term fix

I'm not sure if there is a better way to report the error, or dare I say use the version of python that I installed using `uv`.  I don't know enough to have a POV.

---

_Comment by @Kanerix on 2024-11-13 16:28_

I think your issue is that `uv` is reading the python version from a `.python-version` file in your downloads folder.

That version is not valid (use `uv python list --all-versions` to see valid versions if im not mistaken).

This line shows your issue:
```bash
DEBUG Reading Python requests from version file at `/Users/edmund/Downloads/.python-version`
```

---

_Label `error messages` added by @charliermarsh on 2024-11-13 16:28_

---

_Comment by @charliermarsh on 2024-11-13 16:29_

Thanks for filing. @Kanerix is right that we're reading `3.10-dev` from your `.python-version` file. Then, because it's not a valid version, we assume it's the name of an executable, and we can't find an executable named `3.10-dev`. Agree it would be nice if the error message included more context here.

---

_Referenced in [astral-sh/uv#9092](../../astral-sh/uv/pulls/9092.md) on 2024-11-13 17:15_

---

_Referenced in [astral-sh/uv#12643](../../astral-sh/uv/issues/12643.md) on 2025-04-03 05:40_

---

_Comment by @DavidAntliff on 2025-04-16 02:16_

New to `uv`, this also just caught me out - took me some time to work out what was going on.

I had an existing `~/.python-version` file containing the name of a pyenv virtualenv (my default venv that every shell picks up automatically until I intervene), and `uv` did not like this at all.

Is there a way to add pyenv-managed virtualenvs to `uv` so that it can handle them better?

My workaround for now is to remove `~/.python-version` while I try out `uv`, but it breaks my pyenv set-up which I use all the time, so I'm probably not going to make the switch, unfortunately.

---

_Comment by @zanieb on 2025-04-16 03:52_

Would be fixed by #12909 

---

_Referenced in [xorbitsai/inference#3241](../../xorbitsai/inference/pulls/3241.md) on 2025-04-16 12:44_

---
