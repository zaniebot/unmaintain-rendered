---
number: 13389
title: "`uv` installs an ancient version of `boto3` on Python `3.9`"
type: issue
state: closed
author: peterschmidt85
labels:
  - question
assignees: []
created_at: 2025-05-11T20:06:23Z
updated_at: 2025-05-12T08:21:45Z
url: https://github.com/astral-sh/uv/issues/13389
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv` installs an ancient version of `boto3` on Python `3.9`

---

_Issue opened by @peterschmidt85 on 2025-05-11 20:06_

### Summary

**Steps to reproduce:**

```
git clone https://github.com/dstackai/dstack && cd dstack
uv python pin 3.9
uv sync --all-extras
```

Adding `--refresh` doesn't help.

> Example: https://github.com/dstackai/dstack/actions/runs/14958483893/job/42017331314

**Actual results:**

```
 + boto3==1.7.84
 + botocore==1.10.84
```

**Expected result:**

Should install the latest version.

**Additional notes:**

The same issue as #3143

### Platform

macos-latest

### Version

uv 0.7.3

### Python version

Python 3.9

---

_Label `bug` added by @peterschmidt85 on 2025-05-11 20:06_

---

_Comment by @lagamura on 2025-05-11 21:11_

I suspect that one of your dependencies wrongly requires this old version for python 3.9. When I changed to python>=3.10 the old boto3 was fixed. If that is the case, I can't find directly from `uv.lock` which is the guilty package.

---

_Comment by @charliermarsh on 2025-05-12 00:28_

If you want to guide the resolver to a specific solution, I suggest adding a constraint. uv produces a valid resolution here, but upgrading _other_ packages is requiring it to downgrade boto3.

---

_Label `bug` removed by @charliermarsh on 2025-05-12 00:28_

---

_Label `question` added by @charliermarsh on 2025-05-12 00:28_

---

_Closed by @peterschmidt85 on 2025-05-12 08:21_

---

_Referenced in [haddocking/powerfit#68](../../haddocking/powerfit/pulls/68.md) on 2025-09-16 06:18_

---
