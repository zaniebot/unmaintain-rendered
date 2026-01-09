---
number: 8175
title: "pgcli tool installed using `uv tool install` not working. Do I have to install dependencies separately?"
type: issue
state: closed
author: curioustolearn
labels:
  - question
assignees: []
created_at: 2024-10-14T13:30:25Z
updated_at: 2024-10-16T14:04:15Z
url: https://github.com/astral-sh/uv/issues/8175
synced_at: 2026-01-07T13:12:17-06:00
---

# pgcli tool installed using `uv tool install` not working. Do I have to install dependencies separately?

---

_Issue opened by @curioustolearn on 2024-10-14 13:30_

*uv version*
uv 0.4.20 (Homebrew 2024-10-08)

*OS*
macOS 15.0.1 

*Goal*
To install and use the `pgcli` tool using `uv`. 

*Command run* 
`uv tool install pgcli` and added `~/.local/bin` to path as instructed by uv.

When I try to invoke `pgcli` at the command line (after opening a new terminal window so that the path works), I get the following error:

```
Traceback (most recent call last):
  File "/Users/curiouslearn/.local/bin/pgcli", line 5, in <module>
    from pgcli.main import cli
  File "/Users/curiouslearn/.local/share/uv/tools/pgcli/lib/python3.9/site-packages/pgcli/main.py", line 2, in <module>
    from pgspecial.namedqueries import NamedQueries
  File "/Users/curiouslearn/.local/share/uv/tools/pgcli/lib/python3.9/site-packages/pgspecial/__init__.py", line 12, in <module>
    from . import dbcommands, iocommands
  File "/Users/curiouslearn/.local/share/uv/tools/pgcli/lib/python3.9/site-packages/pgspecial/dbcommands.py", line 7, in <module>
    from psycopg.sql import SQL
  File "/Users/curiouslearn/.local/share/uv/tools/pgcli/lib/python3.9/site-packages/psycopg/__init__.py", line 9, in <module>
    from . import pq  # noqa: F401 import early to stabilize side effects
  File "/Users/curiouslearn/.local/share/uv/tools/pgcli/lib/python3.9/site-packages/psycopg/pq/__init__.py", line 117, in <module>
    import_from_libpq()
  File "/Users/curiouslearn/.local/share/uv/tools/pgcli/lib/python3.9/site-packages/psycopg/pq/__init__.py", line 109, in import_from_libpq
    raise ImportError(
ImportError: no pq wrapper available.
Attempts made:
- couldn't import psycopg 'c' implementation: No module named 'psycopg_c'
- couldn't import psycopg 'binary' implementation: No module named 'psycopg_binary'
- couldn't import psycopg 'python' implementation: libpq library not found
```

Do I have to install dependencies separately?

Thank you. 



---

_Renamed from "pgcli tool installed using uv install not working. Do I have to install dependencies separately?" to "pgcli tool installed using `uv tool install` not working. Do I have to install dependencies separately?" by @curioustolearn on 2024-10-14 13:30_

---

_Comment by @charliermarsh on 2024-10-16 13:37_

It looks like `pgcli` depends on (but does not declare) `pq` being available in some way. Does `uv tool install pgcli --with psycopg-binary` work?

---

_Label `question` added by @charliermarsh on 2024-10-16 13:37_

---

_Comment by @curioustolearn on 2024-10-16 14:02_

Thank you so much @charliermarsh. That worked! I will close the issue. 

---

_Closed by @curioustolearn on 2024-10-16 14:02_

---

_Comment by @charliermarsh on 2024-10-16 14:04_

Awesome, thanks for following up.

---
