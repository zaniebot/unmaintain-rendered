---
number: 19422
title: "N802 false positives for `CGIHTTPRequestHandler` and `SimpleHTTPRequestHandler`"
type: issue
state: closed
author: dscorbett
labels:
  - rule
assignees: []
created_at: 2025-07-18T13:10:00Z
updated_at: 2025-07-23T16:04:12Z
url: https://github.com/astral-sh/ruff/issues/19422
synced_at: 2026-01-10T01:23:00Z
---

# N802 false positives for `CGIHTTPRequestHandler` and `SimpleHTTPRequestHandler`

---

_Issue opened by @dscorbett on 2025-07-18 13:10_

### Summary

#18907 added an exception to [`invalid-function-name` (N802)](https://docs.astral.sh/ruff/rules/invalid-function-name/) for `http.server.BaseHTTPRequestHandler`. That exception should also apply to its subclasses, `http.server.CGIHTTPRequestHandler` and `http.server.SimpleHTTPRequestHandler`. [Example](https://play.ruff.rs/5f94732e-948d-4404-9530-4e7ffffe8706):
```console
$ cat >n802.py <<'# EOF'
from http.server import CGIHTTPRequestHandler, SimpleHTTPRequestHandler
class C(CGIHTTPRequestHandler):
    def do_OPTIONS(self):
        pass
class S(SimpleHTTPRequestHandler):
    def do_OPTIONS(self):
        pass
# EOF

$ ruff --isolated check n802.py --select N802 --output-format concise -q
n802.py:3:9: N802 Function name `do_OPTIONS` should be lowercase
n802.py:6:9: N802 Function name `do_OPTIONS` should be lowercase
```

### Version

ruff 0.12.4 (ee2759b36 2025-07-17)

---

_Label `rule` added by @MichaReiser on 2025-07-18 13:15_

---

_Referenced in [astral-sh/ruff#19432](../../astral-sh/ruff/pulls/19432.md) on 2025-07-19 15:36_

---

_Closed by @ntBre on 2025-07-23 16:04_

---
