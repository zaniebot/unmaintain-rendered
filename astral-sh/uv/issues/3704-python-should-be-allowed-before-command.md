---
number: 3704
title: "`--python` should be allowed before command"
type: issue
state: open
author: thejcannon
labels:
  - compatibility
  - cli
  - breaking
assignees: []
created_at: 2024-05-21T17:00:07Z
updated_at: 2025-07-24T21:01:46Z
url: https://github.com/astral-sh/uv/issues/3704
synced_at: 2026-01-10T01:23:30Z
---

# `--python` should be allowed before command

---

_Issue opened by @thejcannon on 2024-05-21 17:00_

I'm writing some tests that ensure my funky build backend works for several build frontends (`uv` and `pip`).

I make a venv, and want to point `uv` and `pip` to it, however they currently disagree:

`uv` wants it after the command:

```console
$ uv pip --python foo install bar
error: unexpected argument '--python' found

  tip: 'install --python' exists

Usage: uv pip [OPTIONS] <COMMAND>

For more information, try '--help'.
```

`pip` wants it before the command:

```console
$ pip install --python foo bar
ERROR: The --python option must be placed before the pip subcommand name
```

I just want to the pain to stop:
```console
$ make it-stop
make: *** No rule to make target `it-stop'.  Stop.
```

---

_Renamed from "--python should be allowed before command" to "`--python` should be allowed before command" by @thejcannon on 2024-05-21 17:00_

---

_Comment by @zanieb on 2024-05-21 17:07_

Thanks for the report. `--python` isn't currently treated as a "global" `pip` flag. We might be able to change it.. but we'll need to make sure all the `pip` sub-commands support it the same way.

---

_Label `compatibility` added by @zanieb on 2024-05-21 17:07_

---

_Label `cli` added by @zanieb on 2024-05-21 17:07_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-28 18:56_

---

_Referenced in [astral-sh/uv#3889](../../astral-sh/uv/pulls/3889.md) on 2024-05-28 20:44_

---

_Added to milestone `v0.3.0` by @charliermarsh on 2024-06-01 16:58_

---

_Comment by @charliermarsh on 2024-06-01 16:58_

We'll ship this in v0.3.0. It requires changing the `-p` specifier on `uv pip compile`.

---

_Removed from milestone `v0.3.0` by @zanieb on 2024-11-25 20:24_

---

_Added to milestone `v0.6.0` by @zanieb on 2024-11-25 20:24_

---

_Removed from milestone `v0.6.0` by @zanieb on 2025-02-15 02:34_

---

_Added to milestone `v0.7.0` by @zanieb on 2025-02-15 02:34_

---

_Label `breaking` added by @zanieb on 2025-04-21 19:16_

---

_Removed from milestone `v0.7.0` by @zanieb on 2025-04-21 20:14_

---

_Added to milestone `v0.8.0` by @zanieb on 2025-04-21 20:14_

---

_Assigned to @oconnor663 by @zanieb on 2025-06-20 15:11_

---

_Unassigned @charliermarsh by @zanieb on 2025-06-20 15:11_

---

_Removed from milestone `v0.8.0` by @zanieb on 2025-07-24 21:01_

---

_Added to milestone `No target release` by @zanieb on 2025-07-24 21:01_

---
