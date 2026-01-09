---
number: 1616
title: "Extras aren't included for local editable install"
type: issue
state: closed
author: pjbull
labels:
  - duplicate
assignees: []
created_at: 2024-02-17T21:40:32Z
updated_at: 2024-02-18T07:03:10Z
url: https://github.com/astral-sh/uv/issues/1616
synced_at: 2026-01-07T13:12:16-06:00
---

# Extras aren't included for local editable install

---

_Issue opened by @pjbull on 2024-02-17 21:40_

Our [dev requirements file](https://github.com/drivendataorg/cloudpathlib/blob/a242f68d762cdf6a7cfc56e5962a8d83bced5f1b/requirements-dev.txt#L1) has the line `-e .[all]` in it.

While all the other packages seemed to install correctly with `uv pip install -r requirements-dev.txt`, the extras did not.

uv 0.1.1

If you want to repro, you can run:
```bash
git clone https://github.com/drivendataorg/cloudpathlib.git
cd cloudpathlib
uv pip install -r requirements-dev.txt
```

[You should end up with the `azure-storage-blob`, `boto3`, and `google-cloud-storage` packages installed.
](https://github.com/drivendataorg/cloudpathlib/blob/a242f68d762cdf6a7cfc56e5962a8d83bced5f1b/pyproject.toml#L37-L41)

---

_Comment by @charliermarsh on 2024-02-17 21:43_

I believe this was fixed in v0.1.3 -- do you mind trying? (#1435)

---

_Comment by @pjbull on 2024-02-17 22:07_

Yep, looks good in 0.1.3. Sorry for the noise!

---

_Closed by @pjbull on 2024-02-17 22:07_

---

_Comment by @charliermarsh on 2024-02-17 22:10_

No prob, thanks for the clear repro!

---

_Label `duplicate` added by @zanieb on 2024-02-18 07:03_

---
