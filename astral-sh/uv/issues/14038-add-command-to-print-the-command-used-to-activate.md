---
number: 14038
title: Add command to print the command used to activate the virtual environment depending on the current shell
type: issue
state: open
author: Kajiih
labels:
  - enhancement
assignees: []
created_at: 2025-06-14T14:09:31Z
updated_at: 2025-06-14T14:09:31Z
url: https://github.com/astral-sh/uv/issues/14038
synced_at: 2026-01-10T01:25:41Z
---

# Add command to print the command used to activate the virtual environment depending on the current shell

---

_Issue opened by @Kajiih on 2025-06-14 14:09_

### Summary

When running `uv venv`, we get a very helpful message explaining how to activate the virtual environment, e.g., on `Nushell`:
```
> uv venv
...
Activate with: overlay use .venv/bin/activate.nu
```
However, this message can easily get lost in the output if multiple commands are run in sequence (e.g., in shell scripts or install workflows).

It would be practical to have a command that prints this message, it would make it very simple to explain a new user how to activate the environment for its shell.


### Example

After the virtual environment has been created, it could work like this:
```
> uv venv activate-command
overlay use .venv/bin/activate.nu
```

---

_Label `enhancement` added by @Kajiih on 2025-06-14 14:09_

---

_Referenced in [astral-sh/uv#16993](../../astral-sh/uv/issues/16993.md) on 2025-12-05 14:01_

---
