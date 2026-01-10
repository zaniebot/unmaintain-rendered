---
number: 1633
title: Invalid extra name - python_version_2.7_
type: issue
state: closed
author: yoavk
labels:
  - bug
assignees: []
created_at: 2024-02-18T08:21:35Z
updated_at: 2024-02-20T03:26:31Z
url: https://github.com/astral-sh/uv/issues/1633
synced_at: 2026-01-10T01:23:08Z
---

# Invalid extra name - python_version_2.7_

---

_Issue opened by @yoavk on 2024-02-18 08:21_

In addition to [this](https://github.com/astral-sh/uv/issues/1399), there seem to be other extra names that don't match PEP.
I tried installing `uv pip install huaweicloudsdkcore==3.1.56`, but got the following error
```
error: Failed to read metadata for: huaweicloudsdkcore==3.1.56
  Caused by: Failed to parse METADATA file at: huaweicloudsdkcore-3.1.56.dist-info/METADATA
  Caused by: Not a valid package or extra name: "python_version_2.7_". Names must start and end with a letter or digit and may only contain -, _, ., and alphanumeric characters.
```

I found [this](https://gist.github.com/pfmoore/75b2f4528c4e1159c2ab557724b29983#file-extras-txt) list from a few years ago that has a few more extra names that don't match PEP - it could be useful to support (or at least ignore) all these examples.

---

_Label `bug` added by @zanieb on 2024-02-18 08:24_

---

_Comment by @chrisgoddard on 2024-02-18 23:04_

+1 for boto3

---

_Comment by @charliermarsh on 2024-02-20 02:58_

I can look at this.

---

_Referenced in [astral-sh/uv#1731](../../astral-sh/uv/pulls/1731.md) on 2024-02-20 03:15_

---

_Closed by @charliermarsh on 2024-02-20 03:26_

---
