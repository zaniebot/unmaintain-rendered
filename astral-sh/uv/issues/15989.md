```yaml
number: 15989
title: Potential Regression in uv 0.8.19 - uv run - No such file or directory error
type: issue
state: closed
author: jaredmarks-allspring
labels:
  - bug
assignees: []
created_at: 2025-09-22T18:53:05Z
updated_at: 2025-09-22T23:47:48Z
url: https://github.com/astral-sh/uv/issues/15989
synced_at: 2026-01-10T03:23:54Z
```

# Potential Regression in uv 0.8.19 - uv run - No such file or directory error

---

_Issue opened by @jaredmarks-allspring on 2025-09-22 18:53_

### Summary

In the latest version of uv, our docker builds began failing. This is the MRE:

```dockerfile
FROM public.ecr.aws/amazonlinux/amazonlinux:2023-minimal

ENV HOME=/home
ENV UV_NO_CACHE=True UV_LINK_MODE=copy

WORKDIR $HOME

COPY --from=ghcr.io/astral-sh/uv:latest /uv /bin/

RUN uv run -vv --with showcert showcert -i --chain -o pem cdn.amazonlinux.com:443 > /etc/pki/ca-trust/source/anchors/al2023cdn.pem && \
    update-ca-trust
```

-----

Yields:

DEBUG Running `showcert -i --chain -o pem cdn.amazonlinux.com:443`
3.693 DEBUG Released lock at `/tmp/.tmpEZABBx/.lock`
3.705 TRACE Error trace: Failed to spawn: `showcert`
3.705
3.705 Caused by:
3.705     No such file or directory (os error 2)
3.705 error: Failed to spawn: `showcert`
3.705   Caused by: No such file or directory (os error 2)

-----

Reverting to 0.18.8 results in a successful build. Verbose logs attached.

[uv_error_log.txt](https://github.com/user-attachments/files/22475047/uv_error_log.txt)



### Platform

Amazon Linux 2023 Image

### Version

0.8.19

### Python version

3.13

---

_Label `bug` added by @jaredmarks-allspring on 2025-09-22 18:53_

---

_Comment by @zanieb on 2025-09-22 18:58_

This looks the same as https://github.com/astral-sh/uv/issues/15967

Thanks for the report!

---

_Closed by @zanieb on 2025-09-22 20:41_

---

_Comment by @zanieb on 2025-09-22 23:47_

This is fixed in 0.8.20

---
