```yaml
number: 7029
title: "Docker image tag documentation for `uv:python3.12-slim-bookworm` does not match published tag"
type: issue
state: closed
author: zanieb
labels:
  - bug
  - documentation
assignees: []
created_at: 2024-09-04T15:23:19Z
updated_at: 2024-09-04T18:11:59Z
url: https://github.com/astral-sh/uv/issues/7029
synced_at: 2026-01-10T04:45:10Z
```

# Docker image tag documentation for `uv:python3.12-slim-bookworm` does not match published tag

---

_Issue opened by @zanieb on 2024-09-04 15:23_

cc @samypr100 

We transform this to be different than the official image https://github.com/astral-sh/uv/blob/37e25e2b1d0ff78caf9da61380f70f23076bae8f/.github/workflows/build-docker.yml#L174

Any particular reason why? We should either swap that or fix the documentation.

---

_Label `bug` added by @zanieb on 2024-09-04 15:24_

---

_Label `documentation` added by @zanieb on 2024-09-04 15:24_

---

_Comment by @zanieb on 2024-09-04 16:22_

Another option, just create both tags.

---

_Comment by @samypr100 on 2024-09-04 17:59_

> cc @samypr100
> 
> We transform this to be different than the official image
> 
> https://github.com/astral-sh/uv/blob/37e25e2b1d0ff78caf9da61380f70f23076bae8f/.github/workflows/build-docker.yml#L174
> 
> Any particular reason why? We should either swap that or fix the documentation.

The original intent was to match formal debian's upstream pattern which is with -slim standard as a suffix.

`debian:bookworm-slim`

Did I mess up the docs last minute 😅? 



---

_Comment by @zanieb on 2024-09-04 18:11_

It's weird since it doesn't match the Python image tag format but 🤷‍♀️ idk why they do that.

---

_Closed by @zanieb on 2024-09-04 18:11_

---

_Closed by @zanieb on 2024-09-04 18:11_

---
