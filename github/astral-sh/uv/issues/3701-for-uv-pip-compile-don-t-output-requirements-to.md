---
number: 3701
title: "For `uv pip compile` don't output requirements to stdout when output file selected"
type: issue
state: closed
author: notatallshaw-gts
labels:
  - compatibility
  - needs-decision
assignees: []
created_at: 2024-05-21T16:19:34Z
updated_at: 2024-05-21T17:15:29Z
url: https://github.com/astral-sh/uv/issues/3701
synced_at: 2026-01-07T13:12:17-06:00
---

# For `uv pip compile` don't output requirements to stdout when output file selected

---

_Issue opened by @notatallshaw-gts on 2024-05-21 16:19_

Or at least have an option for this? Maybe it already exists and I missed it?

My scenario is I am building a set of requirements with `uv pip compile` via a script and have an output file selected. uv gives some potentially useful warnings, e.g.

```
warning: The requested Python version 3.10.6 is not available; 3.11.4 will be used to build dependencies instead.
Resolved 360 packages in 124ms
warning: The package `google-api-core==2.8.2` does not have an extra named `grpcgcp`.
```

However it's easy to miss these warnings because the screen is quickly then filled with the 360 packages of the output requirements, however I don't need these output requirements on the screen because they are written to a file.

---

_Comment by @zanieb on 2024-05-21 16:22_

Does `-q` work for you?

I believe we omitted this output by default initially but added it to match `pip compile`'s behavior.

---

_Comment by @notatallshaw-gts on 2024-05-21 16:24_

> Does `-q` work for you?

With `-q` I see no messages from uv, including warnings.

> I believe we omitted this output by default initially but added it to match pip compile's behavior.

Fair enough, I'm not that familiar with pip compile's specific behaviors. 

---

_Comment by @charliermarsh on 2024-05-21 16:31_

IDK I'm torn on it. I'd change it back. I find it strange but yes we did it for compatibility.

---

_Label `compatibility` added by @charliermarsh on 2024-05-21 16:31_

---

_Label `needs-decision` added by @charliermarsh on 2024-05-21 16:31_

---

_Comment by @notatallshaw-gts on 2024-05-21 16:57_

FYI I tested with `-q` using `pip-compile` and it displays some, but not all, warnings.

Though it's docs state "-q, --quiet Give less output", where as uv's state "-q, --quiet Do not print any output".

uv's use of `-q` makes more sense to me, I'm sure some users will not want any output.

---

_Comment by @zanieb on 2024-05-21 17:00_

Oh I think the workaround people have used in the past is to pipe stdout to a `/dev/null` since warnings are on stderr and this output is on stdout?

Maybe we should just have a `--no-stdout` flag? hm.

---

_Comment by @notatallshaw-gts on 2024-05-21 17:04_

> Oh I think the workaround people have used in the past is to pipe stdout to a `/dev/null` since warnings are on stderr and this output is on stdout?

Ah thanks, this is sufficient for me!

Feel free to close this issue if you would rather keep compatibility / not add any more flags.

---

_Closed by @charliermarsh on 2024-05-21 17:15_

---
