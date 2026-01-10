```yaml
number: 7138
title: "installation.md: update shell syntax for setting env vars"
type: pull_request
state: merged
author: ilyagr
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-2
created_at: 2024-09-06T21:35:17Z
updated_at: 2024-09-07T15:23:17Z
url: https://github.com/astral-sh/uv/pull/7138
synced_at: 2026-01-10T12:53:41Z
```

# installation.md: update shell syntax for setting env vars

---

_Pull request opened by @ilyagr on 2024-09-06 21:35_

I find this more readable; it also no longer requires bash (thought just replacing `bash` with `sh` would achieve that much).

Strictly speaking, `env` is not necessary on most shells, but I find it makes things clearer.

## Test Plan

Ran the command locally, did not try compiling the docs.



----------

Aside, loosely related (and hopefully helpful) suggestions:

I'm hoping you will also explain in the docs how to install to `~/.local/bin`, with the same goal as https://github.com/astral-sh/uv/pull/6839. Using environment variables for that is fine.

Another minor FR I'd have is to mention these environment variables in the help message of the installer script, especially if you want to encourage people to use them.

Thank you for working on the installation script! It helps me feel more comfortable about eventually asking that people install `uv` to compile docs on my project, hopefully helping people who don't have a Python environment already installed.


---

_Comment by @zanieb on 2024-09-07 15:21_

Thanks this seems reasonable to me! I recently put up https://github.com/astral-sh/uv/pull/7107 — https://github.com/astral-sh/uv/pull/6839 which looked a bit too complicated for users and I've been working with the upstream team to make configuration of the installer simpler / clearer.

> Another minor FR I'd have is to mention these environment variables in the help message of the installer script, especially if you want to encourage people to use them.

Can you open an issue for this so we can track it?

---

_Label `documentation` added by @zanieb on 2024-09-07 15:21_

---

_@zanieb approved on 2024-09-07 15:21_

---

_Merged by @zanieb on 2024-09-07 15:21_

---

_Closed by @zanieb on 2024-09-07 15:21_

---

_Comment by @zanieb on 2024-09-07 15:23_

One small problem with this approach is now the Windows recommendation is a different style :/

---
