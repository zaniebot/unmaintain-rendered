```yaml
number: 12262
title: uv 0.6.7 tls handshake eof
type: issue
state: closed
author: leon1995
labels:
  - bug
assignees: []
created_at: 2025-03-18T08:34:58Z
updated_at: 2025-03-18T17:54:24Z
url: https://github.com/astral-sh/uv/issues/12262
synced_at: 2026-01-10T03:50:31Z
```

# uv 0.6.7 tls handshake eof

---

_Issue opened by @leon1995 on 2025-03-18 08:34_

### Question

When using a https proxy in `uv` version `0.6.7` in combination with a custom index url, I receive the following error
```
uv sync --reinstall
Resolved 116 packages in 5ms
  × Failed to download `uvicorn==0.23.2`
  ├─▶ Failed to fetch:
  │   `https://my-corp-index-url/uvicorn-0.23.2-py3-none-any.whl`
  ├─▶ Request failed after 3 retries
  ├─▶ error sending request for url
  │   (https://my-corp-index-url/uvicorn-0.23.2-py3-none-any.whl)
  ├─▶ client error (Connect)
  ╰─▶ tls handshake eof
  help: `uvicorn` (v0.23.2) was included because `...` depends on `...` (v...) which depends on `uvicorn`
```
This is not a problem in earlier versions of `uv`. I explicitly checked versions `0.6.4`, `0.6.5`, `0.6.6`. I couldn't find any hints in the release notes for `0.6.7` that is related to my problem.

### Platform

windows 11

### Version

0.6.7

---

_Label `question` added by @leon1995 on 2025-03-18 08:34_

---

_Comment by @charliermarsh on 2025-03-18 13:11_

I would assume this is due to updating `reqwest` in #12219.

---

_Comment by @charliermarsh on 2025-03-18 13:34_

Are you able to test a binary if we put up a branch?

---

_Comment by @zanieb on 2025-03-18 13:37_

Are you using proxy exclusions? e.g., as described in:

- https://github.com/seanmonstar/reqwest/issues/1444
- https://github.com/seanmonstar/reqwest/issues/2599
- https://github.com/astral-sh/uv/issues/11640

---

_Label `question` removed by @zanieb on 2025-03-18 13:37_

---

_Label `bug` added by @zanieb on 2025-03-18 13:37_

---

_Comment by @leon1995 on 2025-03-18 14:02_

> Are you able to test a binary if we put up a branch?

yes

> Are you using proxy exclusions? e.g., as described in:
> ...

I have a `NO_PROXY` environment variable where exclusions are configured

---

_Comment by @zanieb on 2025-03-18 14:53_

I suspect this is

- https://github.com/seanmonstar/reqwest/pull/2601
- https://github.com/seanmonstar/reqwest/issues/2599

It should be fixed in the next reqwest release. We could probably release with that commit if we release before them? @charliermarsh 

---

_Closed by @zanieb on 2025-03-18 17:54_

---
