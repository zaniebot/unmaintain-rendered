---
number: 13357
title: "Token is not redacted in \"TRACE Fetching metadata for typer from https://token:ACTUAL_TOKEN@pkgs.shopify.io/basic/data/python/simple/typer/\" log lines"
type: issue
state: closed
author: Ark-kun
labels:
  - bug
assignees: []
created_at: 2025-05-08T23:55:56Z
updated_at: 2025-05-09T21:36:00Z
url: https://github.com/astral-sh/uv/issues/13357
synced_at: 2026-01-10T01:25:32Z
---

# Token is not redacted in "TRACE Fetching metadata for typer from https://token:ACTUAL_TOKEN@pkgs.shopify.io/basic/data/python/simple/typer/" log lines

---

_Issue opened by @Ark-kun on 2025-05-08 23:55_

### Summary

```
TRACE Fetching metadata for typer from https://token:ACTUAL_TOKEN@pkgs.shopify.io/basic/data/python/simple/typer/
```
Meanwhile other lines are redacted:
```
Handling request for https://token:****@pkgs.shopify.io/basic/data/python/simple/cloud-pipelines/ with authentication policy auto
Request for https://token:****@pkgs.shopify.io/basic/data/python/simple/cloud-pipelines/ already contains username and password

```

### Platform

macOS 15

### Version

0.7.2

### Python version

Python 3.11

---

_Label `bug` added by @Ark-kun on 2025-05-08 23:55_

---

_Assigned to @jtfmumm by @konstin on 2025-05-09 07:01_

---

_Comment by @jtfmumm on 2025-05-09 07:11_

#13333 will fix this.

---

_Comment by @jtfmumm on 2025-05-09 07:21_

Duplicate of #1714.

---

_Closed by @jtfmumm on 2025-05-09 07:21_

---

_Reopened by @charliermarsh on 2025-05-09 21:35_

---

_Closed by @charliermarsh on 2025-05-09 21:36_

---
