```yaml
number: 13675
title: Gitlab CI example should quote varaibles to avoid version parsing as integer
type: issue
state: closed
author: AKuederle
labels:
  - bug
  - help wanted
assignees: []
created_at: 2025-05-27T11:45:37Z
updated_at: 2025-05-27T12:43:31Z
url: https://github.com/astral-sh/uv/issues/13675
synced_at: 2026-01-12T16:01:34Z
```

# Gitlab CI example should quote varaibles to avoid version parsing as integer

---

_@AKuederle_

### Summary

In the Gitlab CI example (https://docs.astral.sh/uv/guides/integration/gitlab/) the variables are not quoted and hence, parsed as integers. This causes issues with version numbers with trailing 0 (e.g. Python 3.10, or a future uv 0.10), as the 0 is removed in the when inserted into the template string.

Currently:

```
variables:
  UV_VERSION: 0.5
  PYTHON_VERSION: 3.12
```

Save version:


```
variables:
  UV_VERSION: "0.5"
  PYTHON_VERSION: "3.12"
```

### Platform

Linux

### Version

0.7

### Python version

all Python

---

_Label `bug` added by @AKuederle on 2025-05-27 11:45_

---

_Comment by @charliermarsh on 2025-05-27 12:24_

PR welcome.

---

_Label `help wanted` added by @charliermarsh on 2025-05-27 12:24_

---

_Closed by @charliermarsh on 2025-05-27 12:43_

---
