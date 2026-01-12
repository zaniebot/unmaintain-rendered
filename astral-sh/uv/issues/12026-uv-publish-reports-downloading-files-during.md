```yaml
number: 12026
title: "`uv publish` reports downloading files during publish"
type: issue
state: closed
author: zanieb
labels:
  - bug
  - cli
assignees: []
created_at: 2025-03-06T22:49:15Z
updated_at: 2025-03-07T15:48:36Z
url: https://github.com/astral-sh/uv/issues/12026
synced_at: 2026-01-12T16:00:52Z
```

# `uv publish` reports downloading files during publish

---

_@zanieb_

See https://github.com/astral-sh/uv/actions/runs/13707864294/job/38338658887

```
2025-03-06T21:11:12.3699053Z Uploading uv-0.6.5-py3-none-manylinux_2_17_s390x.manylinux2014_s390x.whl (19.7MiB)
2025-03-06T21:11:12.3699903Z DEBUG Hashing wheels_uv/uv-0.6.5-py3-none-manylinux_2_17_s390x.manylinux2014_s390x.whl
2025-03-06T21:11:12.4453195Z Downloading uv-0.6.5-py3-none-manylinux_2_17_s390x.manylinux2014_s390x.whl (19.7MiB)
2025-03-06T21:11:12.4453982Z DEBUG Using username/password basic auth
2025-03-06T21:11:15.9619082Z DEBUG Response code for https://upload.pypi.org/legacy/: 200 OK
2025-03-06T21:11:15.9619717Z INFO Upload succeeded
```

I believe this is just invoking the reporter incorrectly?

https://github.com/astral-sh/uv/blob/4611690745c2358aaec407bcd620ac1255520255/crates/uv-publish/src/lib.rs#L754

https://github.com/astral-sh/uv/blob/cfd1e670ddb803f4e67d4abd069fad271e1d1c7f/crates/uv/src/commands/reporters.rs#L587-L597

https://github.com/astral-sh/uv/blob/cfd1e670ddb803f4e67d4abd069fad271e1d1c7f/crates/uv/src/commands/reporters.rs#L185

---

_Label `bug` added by @zanieb on 2025-03-06 22:49_

---

_Label `cli` added by @zanieb on 2025-03-06 22:49_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-07 01:25_

---

_Closed by @charliermarsh on 2025-03-07 15:48_

---

_Closed by @charliermarsh on 2025-03-07 15:48_

---
