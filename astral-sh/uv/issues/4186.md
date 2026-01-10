```yaml
number: 4186
title: Socks5 proxy setting and subcommands tab completion.
type: issue
state: closed
author: hongyi-zhao
labels:
  - question
assignees: []
created_at: 2024-06-10T03:27:10Z
updated_at: 2024-06-11T01:53:13Z
url: https://github.com/astral-sh/uv/issues/4186
synced_at: 2026-01-10T05:31:37Z
```

# Socks5 proxy setting and subcommands tab completion.

---

_Issue opened by @hongyi-zhao on 2024-06-10 03:27_

I'm not sure this tool supports the following features:

1. According to [the document](https://github.com/astral-sh/uv#environment-variables), it seems that only http[s] proxies are supported:

> HTTP_PROXY, HTTPS_PROXY, ALL_PROXY: The proxy to use for all HTTP/HTTPS requests.

So, I would like to know whether it can also support socks5 proxy, say, socks5://127.0.0.1:16668
 
2. Subcommands tab completion.

Regards,
Zhao

---

_Renamed from "proxy setting and subcommands tab completion." to "socks5 proxy setting and subcommands tab completion." by @hongyi-zhao on 2024-06-10 04:09_

---

_Renamed from "socks5 proxy setting and subcommands tab completion." to "Socks5 proxy setting and subcommands tab completion." by @hongyi-zhao on 2024-06-10 04:15_

---

_Comment by @charliermarsh on 2024-06-10 17:38_

We do support tab completion. The instructions are basically the same as in Ruff: https://docs.astral.sh/ruff/configuration/#shell-autocompletion.

---

_Comment by @zanieb on 2024-06-10 17:43_

I don't think we support SOCKS5 at all (we don't have the `reqwest` crate `socks` feature turned on). I don't think they support a SOCKS5_PROXY environment variable, but if you're interested in it I'd check in upstream.

---

_Label `question` added by @zanieb on 2024-06-10 17:44_

---

_Comment by @hongyi-zhao on 2024-06-10 23:21_

If you are willing to consider, both `SOCKS5` and `SOCKS5H` should be supported.

---

_Comment by @zanieb on 2024-06-10 23:24_

Can you share more about the use case?

---

_Comment by @hongyi-zhao on 2024-06-10 23:33_

My native proxy is of type `SOCKS5`, and in my specific region, using such a proxy to download dependency packages is more efficient. On the other hand, using `SOCKS5H` can effectively fight DNS pollution.

---

_Comment by @zanieb on 2024-06-11 01:53_

Thank you! We'll track this over in https://github.com/astral-sh/uv/issues/4227 since this one has the tab completion question in it.

---

_Closed by @zanieb on 2024-06-11 01:53_

---
