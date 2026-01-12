```yaml
number: 8099
title: "can't install via github actions anymore"
type: issue
state: closed
author: earonesty
labels: []
assignees: []
created_at: 2023-10-20T20:58:37Z
updated_at: 2023-10-20T21:02:47Z
url: https://github.com/astral-sh/ruff/issues/8099
synced_at: 2026-01-12T15:54:47Z
```

# can't install via github actions anymore

---

_@earonesty_

 • Installing ruff (0.1.1)

  HangupException

  The remote server unexpectedly closed the connection.

  at /opt/hostedtoolcache/Python/3.11.6/x64/lib/python3.11/site-packages/dulwich/protocol.py:215 in read_pkt_line

looks like either (a) actions is filtering it or (b) pypi is

either way, i tried this on many repos... seems to reliably fail.   not sure why yet.

---

_Closed by @earonesty on 2023-10-20 20:59_

---

_Reopened by @earonesty on 2023-10-20 21:00_

---

_Closed by @earonesty on 2023-10-20 21:02_

---
