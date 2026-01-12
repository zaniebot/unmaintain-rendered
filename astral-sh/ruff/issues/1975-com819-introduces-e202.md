```yaml
number: 1975
title: COM819 introduces E202
type: issue
state: closed
author: spaceone
labels: []
assignees: []
created_at: 2023-01-18T22:34:41Z
updated_at: 2023-05-31T16:53:49Z
url: https://github.com/astral-sh/ruff/issues/1975
synced_at: 2026-01-12T15:54:41Z
```

# COM819 introduces E202

---

_@spaceone_

```
$ ruff --fix --select COM819 --isolated --diff foo.py 
--- foo.py
+++ foo.py
@@ -1 +1 @@
-common_udm_options.extend(["--set", "ucsversionend=%s" % (options.ucsversionend,), ])
+common_udm_options.extend(["--set", "ucsversionend=%s" % (options.ucsversionend,) ])
```

pycodestyle:
`foo.py:1:82: E202 whitespace before ']'`

---

_Closed by @charliermarsh on 2023-05-31 16:53_

---
