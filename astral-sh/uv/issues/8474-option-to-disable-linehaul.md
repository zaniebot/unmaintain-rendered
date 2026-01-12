```yaml
number: 8474
title: Option to disable linehaul
type: issue
state: closed
author: eaubin
labels:
  - configuration
  - network
assignees: []
created_at: 2024-10-22T19:14:20Z
updated_at: 2024-11-04T19:37:14Z
url: https://github.com/astral-sh/uv/issues/8474
synced_at: 2026-01-12T15:59:27Z
```

# Option to disable linehaul

---

_@eaubin_

https://github.com/astral-sh/uv/pull/2493 added linehaul metadata to the uv user-agent string. For example,

* Windows GHA: uv/0.1.21 {"installer":{"name":"uv","version":"0.1.21"},"python":"3.12.2","implementation":{"name":"CPython","version":"3.12.2"},"distro":null,"system":{"name":"Windows","release":"2022Server"},"cpu":"AMD64","openssl_version":null,"setuptools_version":null,"rustc_version":null,"ci":true}

I'd like to use uv at work, but this is causing my network admins some grief. Could an option to disable the linehaul metadata be added?


---

_Comment by @zanieb on 2024-10-22 19:23_

Do you know if pip includes an opt-out for this? (just wondering)

---

_Label `configuration` added by @zanieb on 2024-10-22 19:23_

---

_Label `network` added by @zanieb on 2024-10-22 19:23_

---

_Comment by @eaubin on 2024-10-22 19:39_

I believe that's controlled through [$PIP_USER_AGENT_USER_DATA](https://pip.pypa.io/en/stable/user_guide/#using-a-proxy-server)

---

_Comment by @charliermarsh on 2024-10-22 21:31_

I think that's additive; it doesn't look like it disables linehaul.

https://github.com/pypa/pip/blob/6958e28f366f00373343efcd5a7e776a810cb133/src/pip/_internal/network/session.py#L203

---

_Comment by @samypr100 on 2024-10-23 23:46_

iirc pip doesn't allow opt-out either, it's standard telemetry pypi expects

---

_Comment by @eaubin on 2024-11-04 19:37_

My network admins loosened their rules, so its not a requirement for my org anymore. I still think its a good idea to allow opt out (or better yet opt in), but since it's very low priority I'll close the issue.

---

_Closed by @eaubin on 2024-11-04 19:37_

---
