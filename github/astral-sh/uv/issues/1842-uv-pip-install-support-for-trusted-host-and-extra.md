---
number: 1842
title: UV pip install support for --trusted-host and --extra-index-url
type: issue
state: closed
author: Cypher1
labels: []
assignees: []
created_at: 2024-02-22T00:28:49Z
updated_at: 2024-02-22T02:01:13Z
url: https://github.com/astral-sh/uv/issues/1842
synced_at: 2026-01-07T13:12:16-06:00
---

# UV pip install support for --trusted-host and --extra-index-url

---

_Issue opened by @Cypher1 on 2024-02-22 00:28_

OS: Mac OSX
UV platform: unknown
UV version: 0.1.6

Repro suggestion:
```
$ cat req.requirements
--extra-index-url http://pypi.company.com/simple/
--trusted-host pypi.company.com
-r requirements-dev.txt
```

```
$uv pip install -r req.requirements
```

My logs
```
$ uv pip install -r "company-platform.requirements"
error: Unexpected '-', expected '-c', '-e', '-r' or the start of a requirement in `req.requirements`...
```



---

_Comment by @charliermarsh on 2024-02-22 00:44_

Thanks. `--trusted-host` is tracked in https://github.com/astral-sh/uv/issues/1339.

---

_Closed by @charliermarsh on 2024-02-22 00:44_

---

_Comment by @Cypher1 on 2024-02-22 01:36_

Sorry does this mean `--extra-index-url` is not planned or is this just a duplicate?

---

_Comment by @charliermarsh on 2024-02-22 01:39_

We actually already support `--extra-index-url` on both the command-line and in `requirements.txt`!

---

_Comment by @Cypher1 on 2024-02-22 02:00_

Ah! Great. Thank you!

---

_Comment by @charliermarsh on 2024-02-22 02:01_

No prob!

---
