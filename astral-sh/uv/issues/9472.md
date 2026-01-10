```yaml
number: 9472
title: "Allow configuration of HTTPS proxy in the `pyproject.toml`"
type: issue
state: closed
author: zanieb
labels:
  - configuration
assignees: []
created_at: 2024-11-27T15:20:04Z
updated_at: 2026-01-06T17:14:00Z
url: https://github.com/astral-sh/uv/issues/9472
synced_at: 2026-01-10T03:11:32Z
```

# Allow configuration of HTTPS proxy in the `pyproject.toml`

---

_Issue opened by @zanieb on 2024-11-27 15:20_

_No description provided._

---

_Label `configuration` added by @zanieb on 2024-11-27 15:20_

---

_Comment by @Mai0313 on 2024-11-29 08:25_

I believe having `http_proxy` and `https_proxy` settings in `pyproject.toml` is crucial. Most companies, including Mediatek and Qualcomm, restrict direct internet access. However, exceptions exist, such as when installing `pytorch`, which requires a proxy. Sharing proxy URLs within a team of over 15 people can be cumbersome, as members often forget the details.

We previously used Rye, which lacked HTTP proxy support until this [request](https://github.com/astral-sh/rye/issues/215). Therefore, I find this feature essential and highly beneficial for users.

---

_Comment by @Mai0313 on 2024-12-03 02:33_

Another good point about this topic from @samypr100 
For those people who wanna know
https://github.com/astral-sh/uv/issues/9461#issuecomment-2513409606

---

_Comment by @zanieb on 2024-12-03 15:32_

@JamesBonddu please read #9452 

---

_Comment by @JamesBonddu on 2024-12-04 01:38_

> [@JamesBonddu](https://github.com/JamesBonddu) please read [#9452](https://github.com/astral-sh/uv/issues/9452)

thk

---

_Comment by @joshuafc on 2025-03-18 14:46_

@Mai0313 Hi, can /root/.config/uv/uv.toml set proxy only for uv? set `http_proxy` in docker compose file will set proxy for all processes, that is not wanted. 

---

_Comment by @trim21 on 2025-09-09 15:31_

it would be helpful to allow set it in uv config file too

---

_Comment by @bitagoras on 2025-10-09 15:31_

Both `HTTPS_PROXY` and the certificate `SSL_CERT_FILE` should be configurable by the `pyproject.toml` and `uv.toml`. In both conda and pip this can be configured permanently in a config file (in `.condarc` and `pip.ini`) by commands that are included in the managers (`conda config --set`  and `pip config set`). Environment variables, on the other hand, require some extra configuration steps in the operating system. For security reasons I might not want to set the proxy settings system-wide, which enables also other programs to access the outside internet.

---

_Comment by @seemethere on 2025-12-01 22:24_

I submitted https://github.com/astral-sh/uv/pull/16918 to resolve

---

_Closed by @zanieb on 2026-01-06 17:14_

---
