---
number: 16993
title: "`uv activate` could print an error message with the appropriate `source .venv/bin/activate` command"
type: issue
state: open
author: charliermarsh
labels:
  - error messages
assignees: []
created_at: 2025-12-05T05:10:02Z
updated_at: 2026-01-06T17:09:35Z
url: https://github.com/astral-sh/uv/issues/16993
synced_at: 2026-01-10T01:26:12Z
---

# `uv activate` could print an error message with the appropriate `source .venv/bin/activate` command

---

_Issue opened by @charliermarsh on 2025-12-05 05:10_

A suggestion that came up in an in-person conversation. Even if we can't support `uv activate`, we could at least print a message telling the user how to activate it.

---

_Label `error messages` added by @charliermarsh on 2025-12-05 05:10_

---

_Comment by @nooscraft on 2025-12-05 09:41_

```
$ uv activate
error No virtual environment found in the current directory or any parent directory.

hint Create a virtual environment with: uv venv
```
Something like above ^^ @charliermarsh 

---

_Comment by @nooscraft on 2025-12-05 13:29_

## Example Outputs

### When a virtual environment exists:
```bash
$ uv activate
Activate with: source .venv/bin/activate
```

### When no virtual environment is found:
```bash
$ uv activate
error No virtual environment found in the current directory or any parent directory.

hint Create a virtual environment with: uv venv
```

### Help output:
```bash
$ uv activate --help
Show how to activate a virtual environment

Usage: uv activate [OPTIONS]
...
```

@charliermarsh FYI!

---

_Comment by @zanieb on 2025-12-05 14:01_

This has already come up a few times before and is very similar to https://github.com/astral-sh/uv/issues/14038.

I'm pretty wary to add a top-level command that just prints the activation command.

---

_Comment by @charliermarsh on 2025-12-05 14:03_

I don’t think it has to be command. Can’t it just be a more helpful error message?

---

_Comment by @nooscraft on 2025-12-05 14:31_

There's no command

When uv activate is run and a venv exists:
  ```
$ uv activate  
  Activate with: source .venv/bin/activate
```

When uv activate is run and no venv exists:
```
 $ uv activate  
error No virtual environment found in the current directory or any parent directory.  hint Create a virtual environment with: uv venv
```

---

_Referenced in [astral-sh/uv#17001](../../astral-sh/uv/pulls/17001.md) on 2025-12-05 16:05_

---

_Comment by @disouzam on 2025-12-31 00:43_

I'm pretty new to uv. Had looked through the documentation now (and some weeks before) and now in this issue and also in #14038. However, I couldn't answer the question: what do you people use to activate a recently created environment? The old `source {os-dependent path}/activate` (in bash, for example)? Or another command?





---

_Comment by @konstin on 2026-01-06 16:59_

With `uv run`, you don't need to activate environments by hand. Otherwise, the old `source .venv/bin/activate` remains working.

---

_Comment by @disouzam on 2026-01-06 17:09_

Understood, @konstin! Thanks! I ended up creating an alias in bash for the old command. 

---
