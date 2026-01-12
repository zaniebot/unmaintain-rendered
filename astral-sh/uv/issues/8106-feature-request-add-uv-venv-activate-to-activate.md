```yaml
number: 8106
title: "[Feature Request] Add `uv venv --activate` to activate virtual environment after creation"
type: issue
state: closed
author: imbev
labels: []
assignees: []
created_at: 2024-10-10T19:47:38Z
updated_at: 2024-10-24T12:23:37Z
url: https://github.com/astral-sh/uv/issues/8106
synced_at: 2026-01-12T15:59:19Z
```

# [Feature Request] Add `uv venv --activate` to activate virtual environment after creation

---

_@imbev_

I suggest that uv add an `--activate` option to the `uv venv` command. This option would automatically activate the virtual environment after creating it.

The following would be mostly equivalent:

```bash
uv venv
source .venv/bin/activate # Or shell equivalent
```

```bash
uv venv --activate
```

Currently, `uv venv` has the following output:

```
Using Python 3.12.6 interpreter at: /usr/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
```

This should be replaced by the following, but only with `--activate`:
```
Using Python 3.12.6 interpreter at: /usr/bin/python3
Creating virtualenv at: .venv
Activating virtualenv at: .venv
```

---

_Comment by @zanieb on 2024-10-10 19:51_

Unfortunately we can't do this because of how shells work. You need to run the activation command from your shell, a subprocess cannot mutate your shell.

---

_Closed by @charliermarsh on 2024-10-10 20:24_

---

_Comment by @Werni2A on 2024-10-10 20:47_

Something related to this request that is probably not worth creating a separate issue.

---

Remembering the activation script location is confusing when switching between Windows and Linux systems. Windows makes it even worse with separate scripts for PowerShell and Command Prompt.

E.g.

- `foobar\Scripts\activate.ps1` (Windows PowerShell)
- `foobar\Scripts\activate` (Windows Command Prompt)
- `foobar/bin/activate` (Linux)

Sometimes it would be useful get the activation script path directly from `uv` to call or source it. E.g. with a imaginary argument `--activation-script`.

```bash
source $(uv venv foobar --activation-script)
# Leading to
# source "foobar/bin/activate"
```

Since `uv` already provides the exact command to activate the environment it's just copy and paste in an interactive environment. I.e. the benefit would be negligible but could make scripts a bit more consistent across shell implementations.

Nothing I personally require - just something that came to mind while reading this post.

---

_Comment by @Ravencentric on 2024-10-10 21:10_

This is also what [PDM went with](https://pdm-project.org/en/latest/usage/venv/#activate-a-virtualenv), i.e., a command to print the activate command for your current OS and shell.

---

_Comment by @Kilo59 on 2024-10-23 22:27_

@zanieb 
Both `poetry` and `pipenv` have a `shell` commands that works equivalent to this proposal.
Why can't `uv` do something similar?

---

_Comment by @Ravencentric on 2024-10-24 06:36_

Poetry is removing their `shell` command in the next major update. https://github.com/python-poetry/poetry/issues/9136

---

_Comment by @zanieb on 2024-10-24 12:23_

See https://github.com/astral-sh/uv/issues/1910

---
