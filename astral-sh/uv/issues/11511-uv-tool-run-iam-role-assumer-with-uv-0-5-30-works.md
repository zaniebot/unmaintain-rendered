```yaml
number: 11511
title: "uv tool run iam_role_assumer with uv 0.5.30 works & 0.5.31 fails"
type: issue
state: closed
author: snowman2
labels:
  - bug
assignees: []
created_at: 2025-02-14T15:28:01Z
updated_at: 2025-02-14T21:53:03Z
url: https://github.com/astral-sh/uv/issues/11511
synced_at: 2026-01-12T16:00:39Z
```

# uv tool run iam_role_assumer with uv 0.5.30 works & 0.5.31 fails

---

_@snowman2_

### Summary

This is the process to reproduce the error with output:

```bash
uv tool install iam_role_assumer
```
```
Resolved 9 packages in 83ms
Downloading botocore (12.7MiB)
 Downloaded botocore
Prepared 9 packages in 221ms
Installed 9 packages in 145ms
 + boto3==1.36.20
 + botocore==1.36.20
 + click==8.1.8
 + iam-role-assumer==0.2.0
 + jmespath==1.0.1
 + python-dateutil==2.9.0.post0
 + s3transfer==0.11.2
 + six==1.17.0
 + urllib3==2.3.0
Installed 1 executable: iam_role_assumer
```
```bash
$(uv tool run iam_role_assumer assume -r arn:aws:iam::{aws_account_id}:role/{role_name})
```
```
warning: An executable named `iam-role-assumer` is not provided by package `iam-role-assumer`.
The: command not found
```

After downgrading to `uv 0.5.30` it works without any issue.

### Platform

Ubuntu 22 x86_64

### Version

0.5.31

### Python version

Python 3.11

---

_Label `bug` added by @snowman2 on 2025-02-14 15:28_

---

_Comment by @zanieb on 2025-02-14 15:32_

Fixed already in #11329 will be out today

---

_Comment by @snowman2 on 2025-02-14 15:48_

Thanks!

---

_Closed by @snowman2 on 2025-02-14 15:48_

---

_Reopened by @snowman2 on 2025-02-14 21:37_

---

_Comment by @snowman2 on 2025-02-14 21:37_

The issue is still present in version 0.6.0.

---

_Comment by @charliermarsh on 2025-02-14 21:38_

I just fixed this in #11524.

---

_Closed by @charliermarsh on 2025-02-14 21:38_

---

_Comment by @snowman2 on 2025-02-14 21:53_

Thanks!

---
