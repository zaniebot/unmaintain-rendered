```yaml
number: 14215
title: Unsure what terminal should look like when uv venv is activated
type: issue
state: open
author: oliverangelil
labels:
  - question
assignees: []
created_at: 2025-06-23T12:42:10Z
updated_at: 2025-08-21T12:56:38Z
url: https://github.com/astral-sh/uv/issues/14215
synced_at: 2026-01-10T03:32:45Z
```

# Unsure what terminal should look like when uv venv is activated

---

_Issue opened by @oliverangelil on 2025-06-23 12:42_

### Question

I'm seeing two different terminal prompts: 
- 1) @oliverangelil ➜ /workspaces/AIMS_course (after typing `deactivate`)
- 2) (AIMS_course) @oliverangelil ➜ /workspaces/AIMS_course (after typing `source .venv/bin/activate`)

![Image](https://github.com/user-attachments/assets/c20b8c4a-a815-4311-aeb2-2ab259478866)

However in both cases it seems the UV .venv is active....
I.e. both list the same active python interpreter, and both list the same packages when `uv pip list` is typed (not shown in screenshot)

Why is the venv active even after typing `deactivate`?

### Platform

Debian GNU/Linux 12 (bookworm)

### Version

uv 0.7.13

---

_Label `question` added by @oliverangelil on 2025-06-23 12:42_

---

_Comment by @nataziel on 2025-06-25 23:34_

re: `uv pip list`, it is expected that it will list the same packages whether the venv is active or not, that command specifically lists the packages installed in the detected uv project. I was also confused by this a while ago but it was explained pretty well here: https://github.com/astral-sh/uv/issues/9111

You might need to investigate the values of the `VIRTUAL_ENV` & `PATH` environment variables before/after activation/deactivation of the environment to work out why it's finding the same python executable

---

_Comment by @konstin on 2025-08-21 12:56_

> I.e. both list the same active python interpreter, and both list the same packages when `uv pip list` is typed (not shown in screenshot)

In this case the venv isn't active, but `uv pip list` reads `.venv` by default, matching how `uv pip install` installs into `.venv` when when the venv isn't activated.

---
