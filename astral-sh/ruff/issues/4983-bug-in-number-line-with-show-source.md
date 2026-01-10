---
number: 4983
title: Bug in number line with --show-source
type: issue
state: closed
author: victorcrrd
labels:
  - bug
assignees: []
created_at: 2023-06-09T13:32:26Z
updated_at: 2023-06-09T15:21:20Z
url: https://github.com/astral-sh/ruff/issues/4983
synced_at: 2026-01-10T01:22:44Z
---

# Bug in number line with --show-source

---

_Issue opened by @victorcrrd on 2023-06-09 13:32_

When printing the code fragment with a violation with the `--show-source` option, it always happens (unless it's on the first line as I can see) that the number at the beginning of each source line doesn't match the actual line number. For example, here
```
src/dropboxAPI.py:2:26: F401 [*] `urllib.parse.urlencode` imported but unused
          |
        2 | from urllib import request
        3 | from urllib.parse import urlencode
          |                          ^^^^^^^^^ F401
        4 | import requests, os
        5 | from base64 import b64encode
          |
          = help: Remove unused import: `urllib.parse.urlencode`
```
I have a violation at line 2, but the number line printed in the source-code fragment is 3.

However, it doesn't happen when the violation is on the first line:
```
src/dropboxAPI.py:1:20: F401 [*] `urllib.request` imported but unused
          |
        1 | from urllib import request
          |                    ^^^^^^^ F401
        2 | from urllib.parse import urlencode
        3 | import requests, os
          |
          = help: Remove unused import: `urllib.request`
```

---

_Label `bug` added by @charliermarsh on 2023-06-09 13:34_

---

_Assigned to @MichaReiser by @MichaReiser on 2023-06-09 13:53_

---

_Comment by @MichaReiser on 2023-06-09 14:00_

Thanks for reporting this bug. It's a bit embarrassing, the line numbers are even off in our test snapshots. I take a look (how to catch up with @charliermarsh 's insane added/removed stats: mess up the line numbers so that all snapshot tests need updating)

---

_Closed by @MichaReiser on 2023-06-09 15:21_

---
