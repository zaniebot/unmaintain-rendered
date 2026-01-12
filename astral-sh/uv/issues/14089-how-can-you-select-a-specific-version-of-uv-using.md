```yaml
number: 14089
title: "How can you select a specific version of `uv` using `install.sh`?"
type: issue
state: closed
author: johnthagen
labels:
  - question
assignees: []
created_at: 2025-06-16T19:46:27Z
updated_at: 2025-06-16T19:56:19Z
url: https://github.com/astral-sh/uv/issues/14089
synced_at: 2026-01-12T16:01:43Z
```

# How can you select a specific version of `uv` using `install.sh`?

---

_@johnthagen_

### Question

I'd like to use `curl -LsSf https://astral.sh/uv/install.sh` to install `uv`, but do it in a way where I can control which version of `uv` is installed, either through an environment variable `UV_VERSION`, etc. or a CLI flag.

I could not find this at https://docs.astral.sh/uv/reference/installer/

or when running

```
curl -LsSf https://astral.sh/uv/install.sh | sh -s -- --help
```

### Platform

Rocky Linux 8

### Version

uv 0.7.13

---

_Label `question` added by @johnthagen on 2025-06-16 19:46_

---

_Comment by @charliermarsh on 2025-06-16 19:47_

You can put any version in the URL, like:

```
curl -LsSf https://astral.sh/uv/0.6.0/install.sh | sh
```

---

_Comment by @zanieb on 2025-06-16 19:55_

Note this is covered in https://docs.astral.sh/uv/getting-started/installation/#standalone-installer

---

_Closed by @zanieb on 2025-06-16 19:55_

---

_Comment by @johnthagen on 2025-06-16 19:56_

Ugh, sorry I don't know how I missed that 🤦 .

Thanks!

---
