```yaml
number: 2988
title: uv does not recognise PIP_BREAK_SYSTEM_PACKAGES env variable
type: issue
state: closed
author: m-reichle
labels:
  - compatibility
  - configuration
assignees: []
created_at: 2024-04-11T07:42:25Z
updated_at: 2024-04-12T07:00:36Z
url: https://github.com/astral-sh/uv/issues/2988
synced_at: 2026-01-12T15:58:41Z
```

# uv does not recognise PIP_BREAK_SYSTEM_PACKAGES env variable

---

_@m-reichle_

Hi, 

I am trying to use uv as a drop-in replacement for pip in a test container setting to speed up my test runs. 
Since my containers are built new for every test run, I don't use a venv and allow pip to break system packages. I do that by setting PIP_BREAK_SYSTEM_PACKAGES=1 in my environment, rather than using the break system packages argument on every pip call.

It seems uv doesn't recognise that environment variable though. Would it be possible to add it?  

---

_Comment by @m-reichle on 2024-04-11 08:11_

For reference: pip recognises env variables for all its CLI arguments by checking for `PIP_<UPPER_LONG_NAME>`. See https://pip.pypa.io/en/stable/topics/configuration/#environment-variables

---

_Comment by @charliermarsh on 2024-04-11 14:53_

As a "rule", we don't read pip-specific configuration, but we do read `UV_<ENV_VAR>` for some values: https://github.com/astral-sh/uv?tab=readme-ov-file#environment-variables.

I don't mind adding `UV_BREAK_SYSTEM_PACKAGES` there.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-11 14:53_

---

_Label `compatibility` added by @charliermarsh on 2024-04-11 14:53_

---

_Label `configuration` added by @charliermarsh on 2024-04-11 14:53_

---

_Closed by @charliermarsh on 2024-04-11 15:58_

---

_Comment by @m-reichle on 2024-04-12 07:00_

Aw, awesome! Thank you very much for the quick response!

---
