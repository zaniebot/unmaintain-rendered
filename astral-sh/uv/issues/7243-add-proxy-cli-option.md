```yaml
number: 7243
title: Add --proxy CLI option
type: issue
state: closed
author: IvanciuVlad
labels:
  - question
assignees: []
created_at: 2024-09-10T06:38:52Z
updated_at: 2024-10-21T21:50:00Z
url: https://github.com/astral-sh/uv/issues/7243
synced_at: 2026-01-12T15:59:11Z
```

# Add --proxy CLI option

---

_@IvanciuVlad_

Platform: Windows 11
uv version: 0.4.7

**WHAT**
Allow setting the proxy in the CLI similar to how pip allows you too.

**WHY**
For us, uv is being blocked by the corporate proxy, and we cannot set the correct proxy as environmental variables due to rights issues as the docs suggest.

**HOW**

- uv --proxy=http://**.**.**.**:port add pandas

---

_Comment by @charliermarsh on 2024-09-10 13:24_

What is the use-case for a proxy as opposed to setting the index URL via `--index-url`?

---

_Label `question` added by @charliermarsh on 2024-09-10 13:24_

---

_Comment by @notatallshaw on 2024-09-10 15:50_

FYI `--proxy` on pip side is to point to a HTTP proxy, not a package index proxy.

---

_Comment by @IvanciuVlad on 2024-09-22 16:26_

Hi, sorry for long delay in response. Yes, we would need http proxy, otherwise the corporate network will block allow us from installing packages.

---

_Comment by @YangNianYi on 2024-10-14 09:03_

`reqwest`: System proxies are enabled by default.
set environment variables
`HTTP_PROXY`
`HTTPS_PROXY`
`ALL_PROXY`
https://github.com/seanmonstar/reqwest/blob/master/src/lib.rs#L138


---

_Closed by @zanieb on 2024-10-21 21:50_

---
