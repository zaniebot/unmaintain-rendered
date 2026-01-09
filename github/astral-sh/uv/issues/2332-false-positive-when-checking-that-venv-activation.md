---
number: 2332
title: False positive when checking that venv activation script is being sourced
type: issue
state: closed
author: astoff
labels:
  - needs-decision
assignees: []
created_at: 2024-03-10T12:02:17Z
updated_at: 2024-03-10T18:48:20Z
url: https://github.com/astral-sh/uv/issues/2332
synced_at: 2026-01-07T13:12:17-06:00
---

# False positive when checking that venv activation script is being sourced

---

_Issue opened by @astoff on 2024-03-10 12:02_

Suppose the user is not physically typing `source .venv/bin/activate` in an interactive shell, but instead has some automation in place which runs `bash -c 'source "$0" && env' .venv/bin/activate` and parses the output of that process.  Then the following check will trigger an error:

https://github.com/astral-sh/uv/blame/6dcd00e0315c0b4ef743d7609c517b90fa607962/crates/uv-virtualenv/src/activator/activate#L26-L29

Perhaps it would make more sense to replace the test with `if [ -z "${BASH_SOURCE-}" ]; then ...`?

---

_Comment by @charliermarsh on 2024-03-10 12:23_

I think this script is just identical to that used in other virtualenv creators: https://github.com/pypa/virtualenv/blob/abe2c998bf8c7593a3a3ed2c19b3ebd55675c818/src/virtualenv/activation/bash/activate.sh#L5

---

_Comment by @charliermarsh on 2024-03-10 12:47_

I would be hesitant to modify it without also seeing that modification in virtualenv. Can you see if there have ever been similar requests there?

---

_Label `needs-decision` added by @charliermarsh on 2024-03-10 12:47_

---

_Comment by @astoff on 2024-03-10 13:01_

Actually I wrote this issue because those lines seem to have been added in uv, compare with https://github.com/python/cpython/blob/main/Lib/venv/scripts/common/activate.

---

_Comment by @charliermarsh on 2024-03-10 13:30_

I saw that! But since it exists in virtualenv, I'd want to understand why and how it was tested in virtualenv before making any changes.

---

_Comment by @astoff on 2024-03-10 17:30_

Okay, digging a bit more, I found the following clarification in the bash manpage:

> If the -c option is present, then commands are read from the
                 first non-option argument command_string.  If there are
                 arguments after the command_string, the **first argument is
                 assigned to $0** and any remaining arguments are assigned to
                 the positional parameters.  **The assignment to $0 sets the
                 name of the shell, which is used in warning and error
                 messages.**

So I'd say that the check in the activate script is sane, and my original code example is flawed (one should use `bash -c 'source "$1" && env' bash .venv/bin/activate` or something similar). So I'll close this issue.

---

_Closed by @astoff on 2024-03-10 17:30_

---

_Comment by @charliermarsh on 2024-03-10 18:48_

Thank you!

---
