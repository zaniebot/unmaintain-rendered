---
number: 8522
title: "the message  \"or pass `--system` to install into a non-virtual environment\" needs greater detail or is wrong. or something"
type: issue
state: closed
author: twobob
labels:
  - question
assignees: []
created_at: 2024-10-24T10:55:50Z
updated_at: 2024-10-24T15:23:23Z
url: https://github.com/astral-sh/uv/issues/8522
synced_at: 2026-01-07T13:12:17-06:00
---

# the message  "or pass `--system` to install into a non-virtual environment" needs greater detail or is wrong. or something

---

_Issue opened by @twobob on 2024-10-24 10:55_



d:\dev\genmoai-smol>`uv pip install somethingoutsideanenv`
error: No virtual environment found; run `uv venv` to create an environment, or pass `--system` to install into a non-virtual environment

Give the above error output one would most likely do the following:

d:\dev\genmoai-smol>`uv --system pip install somethingoutsideanenv`
error: unexpected argument '--system' found

Usage: `uv [OPTIONS] <COMMAND>`

For more information, try '--help'.


it says `UV [OPTIONS] <COMMAND>`

and yet

**`uv pip install sageattention --system`**  is the command which is not at all  `UV [OPTIONS] <COMMAND>`

Can someone explain why this is the case

thank you Great Sages
<img src="https://github.com/user-attachments/assets/1bcde921-01c5-4ca0-85c2-55921414d79a" alt="greatSage" width="300" height="300">


---

_Comment by @charliermarsh on 2024-10-24 13:52_

Yeah you need to pass `--system` after the command (like `uv pip install --system sageattention` -- it just has to come after `uv pip install`). `--system` is an argument to `uv pip install` (the subcommand), not `uv` (the top-level command).

`uv [OPTIONS] <COMMAND>` is shown because there _are_ some arguments that are global to `uv`. For example, `uv --verbose pip install sageattention` is fine, because `--verbose` is a global argument.

---

_Label `question` added by @charliermarsh on 2024-10-24 13:52_

---

_Comment by @twobob on 2024-10-24 14:12_

Thanks Charlie. perhaps I should have been more specific

`error: No virtual environment found; run uv venv to create an environment, or pass --system **AFTER ALL THE COMMANDS** to install into a non-virtual environment
`

would make more sense as a message.  Since it is an exception as you have noted.


---

_Closed by @twobob on 2024-10-24 14:12_

---

_Comment by @zanieb on 2024-10-24 14:18_

Generally, I wouldn't recommend passing any arguments immediately after `uv ...` when using subcommands. It's more normal to just provide them all at the end.

---

_Comment by @twobob on 2024-10-24 15:23_

> Generally, I wouldn't recommend passing any arguments immediately after `uv ...` when using subcommands. It's more normal to just provide them all at the end.

Fair,

---
