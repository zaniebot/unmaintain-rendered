```yaml
number: 3648
title: Support UV_CONFIG env variable to provide path to global configuration file
type: issue
state: closed
author: kpfleming
labels:
  - documentation
assignees: []
created_at: 2024-05-18T13:28:18Z
updated_at: 2024-05-19T02:25:34Z
url: https://github.com/astral-sh/uv/issues/3648
synced_at: 2026-01-10T05:31:37Z
```

# Support UV_CONFIG env variable to provide path to global configuration file

---

_Issue opened by @kpfleming on 2024-05-18 13:28_

When building a container image with some pre-configuration for uv, I'd like to be able to run commands later without having to specify the path to the configuration file... and the container image doesn't have 'user home directories' so the usual discovery process isn't applicable.

I'd like to be able to set a UV_CONFIG (or similar name) environment variable which would bypass the discovery process and use the provided path for uv.toml directly.

---

_Comment by @charliermarsh on 2024-05-18 13:47_

We actually already support this! Try `UV_CONFIG_FILE`. I need to add to the list in the README.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-18 13:47_

---

_Label `documentation` added by @charliermarsh on 2024-05-18 13:47_

---

_Closed by @zanieb on 2024-05-19 02:25_

---
