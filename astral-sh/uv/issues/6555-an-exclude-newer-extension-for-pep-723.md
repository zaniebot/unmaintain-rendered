```yaml
number: 6555
title: An exclude newer extension for PEP 723
type: issue
state: closed
author: hauntsaninja
labels:
  - documentation
  - question
assignees: []
created_at: 2024-08-23T22:30:13Z
updated_at: 2024-10-21T21:18:11Z
url: https://github.com/astral-sh/uv/issues/6555
synced_at: 2026-01-10T04:45:09Z
```

# An exclude newer extension for PEP 723

---

_Issue opened by @hauntsaninja on 2024-08-23 22:30_

This would be a one liner you could add that to make most scripts 99% reproducible using uv's awesome `--exclude-newer`

---

_Comment by @hauntsaninja on 2024-08-23 22:33_

I guess two liner because it would probably need to be under `[tool.uv]`

---

_Comment by @zanieb on 2024-08-23 22:38_

You should already be able to do this!

---

_Comment by @charliermarsh on 2024-08-23 22:40_

Yeah I believe this works:

```toml
# /// script
# requires-python = ">=3.12"
# dependencies = [
#     "flask",
# ]
#
# [tool.uv]
# exclude-newer = "2023-03-25T00:00:00Z"
# ///
```

---

_Comment by @zanieb on 2024-08-23 22:40_

e.g.

```
# /// script
# requires-python = ">=3.11"
# dependencies = [
#     "httpx",
# ]
#
# [tool.uv]
# exclude-newer = "2022-07-17T00:00:00Z"
# ///
```

gives 

```
 + anyio==3.6.1
 + certifi==2022.6.15
 + h11==0.12.0
 + httpcore==0.15.0
 + httpx==0.23.0
 + idna==3.3
 + rfc3986==1.5.0
 + sniffio==1.2.0
```

and without

```
 + anyio==4.4.0
 + certifi==2024.7.4
 + h11==0.14.0
 + httpcore==1.0.5
 + httpx==0.27.0
 + idna==3.8
 + sniffio==1.3.1
```

---

_Label `question` added by @zanieb on 2024-08-23 22:40_

---

_Label `documentation` added by @zanieb on 2024-08-23 22:41_

---

_Comment by @hauntsaninja on 2024-08-23 23:37_

Ah, I should have tried it! https://github.com/astral-sh/uv/pull/6562 adds some things that would be useful :-)

---

_Closed by @zanieb on 2024-10-21 21:18_

---
