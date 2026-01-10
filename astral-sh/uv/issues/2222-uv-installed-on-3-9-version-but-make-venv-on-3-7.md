```yaml
number: 2222
title: uv installed on 3.9 version, but make venv on 3.7 version
type: issue
state: closed
author: mephistiks
labels:
  - enhancement
assignees: []
created_at: 2024-03-05T21:53:40Z
updated_at: 2024-05-22T16:34:26Z
url: https://github.com/astral-sh/uv/issues/2222
synced_at: 2026-01-10T05:31:37Z
```

# uv installed on 3.9 version, but make venv on 3.7 version

---

_Issue opened by @mephistiks on 2024-03-05 21:53_

Input:
```sh 
python3.9 -m uv venv venv
```

Output:
```sh
Using Python 3.7.17 interpreter at: /usr/bin/python3
Creating virtualenv at: venv
Activate with: source venv/bin/activate
```

---

_Comment by @charliermarsh on 2024-03-05 21:55_

I would recommend instead: `uv venv --python python3.9`.

---

_Comment by @mephistiks on 2024-03-05 21:56_

Amazing, now it is working

---

_Closed by @mephistiks on 2024-03-05 21:56_

---

_Comment by @zanieb on 2024-03-06 00:02_

We _could_ hint to UV to use the correct version as we do for virtual environments, I would expect this command to work as well.

https://github.com/astral-sh/uv/blob/65518c9c585648267721a6effc090cb2f18e12dc/python/uv/__main__.py#L29-L31

---

_Reopened by @zanieb on 2024-03-06 00:02_

---

_Label `bug` added by @zanieb on 2024-03-06 00:02_

---

_Comment by @charliermarsh on 2024-03-06 00:03_

Perhaps, take a look at https://github.com/astral-sh/uv/issues/2058 too.

---

_Label `bug` removed by @charliermarsh on 2024-03-08 20:55_

---

_Label `needs-decision` added by @charliermarsh on 2024-03-08 20:55_

---

_Assigned to @zanieb by @zanieb on 2024-05-22 13:32_

---

_Label `needs-decision` removed by @zanieb on 2024-05-22 13:32_

---

_Label `enhancement` added by @zanieb on 2024-05-22 13:32_

---

_Closed by @zanieb on 2024-05-22 16:34_

---
