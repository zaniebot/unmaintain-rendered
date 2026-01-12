```yaml
number: 7579
title: uv hangs when PyPI is timing out, even when installing a local wheel with all dependencies being met
type: issue
state: open
author: ThiefMaster
labels:
  - enhancement
assignees: []
created_at: 2024-09-20T10:29:53Z
updated_at: 2024-09-24T15:28:33Z
url: https://github.com/astral-sh/uv/issues/7579
synced_at: 2026-01-12T15:59:15Z
```

# uv hangs when PyPI is timing out, even when installing a local wheel with all dependencies being met

---

_@ThiefMaster_

A few days ago, `uv pip install ...` just got stuck for me because `https://pypi.org/simple/lxml/` was timing out.

I was reinstalling a package from a local wheel file, and the required lxml version (and all other deps) were already installed, so I'm not sure why it even needs to perform any network requests there at all.

`--no-deps` worked to avoid hanging, but it feels like a bug that an unresponsive PyPI server breaks anything in this particular case.

In case it's useful (I doubt it, since the problem is likely the fact that the response takes a long time and not that it's an error response), this is what PyPI replied on that day:

```
$ http -phHbB get https://pypi.org/simple/lxml/
GET /simple/lxml/ HTTP/1.1
Accept: */*
Accept-Encoding: gzip, deflate
Connection: keep-alive
Host: pypi.org
User-Agent: HTTPie/3.2.3



HTTP/1.1 503 Timed out while waiting
Connection: close
Content-Type: text/plain;charset=US-ASCII
Fastly-Host: cache-par-lfpg1960051-PAR
Server: Varnish
date: Mon, 16 Sep 2024 22:45:11 GMT
transfer-encoding: chunked

Timed out while waiting on cache-par-lfpg1960051-PAR
```

---

_Comment by @charliermarsh on 2024-09-24 12:21_

Likely because we need to get the latest list of versions from the registry. Even if something is cached, we generally need to do this, since there could be a newer version of a given dependency available on the registry.

---

_Label `question` added by @charliermarsh on 2024-09-24 12:21_

---

_Comment by @ThiefMaster on 2024-09-24 12:22_

Why does it matter if there's a newer version when I don't ask uv to upgrade anything via `-U`?

---

_Comment by @charliermarsh on 2024-09-24 12:53_

I can come up with examples where it would be necessary but it depends on more context. Can you explain in more detail the command you ran, and the state of the environment at the time? If lxml is installed and you run uv pip install lxml, we shouldn’t be hitting the registry at all.

---

_Comment by @ThiefMaster on 2024-09-24 12:59_

I built an updated wheel of my application (w/o touching any of its dependencies which are *all* pinned to exact versions), and wanted to install that in my env (where the previous version of my wheel was already installed) using `uv pip install myapp-whatever.whl`

---

_Comment by @charliermarsh on 2024-09-24 15:28_

I think it's probably possible to make that succeed (especially with pinned versions), though it would require some non-trivial refactors (we'd have to move from "get package metadata, then version metadata" to "get version metadata if already installed, continue if that version works, otherwise get package metadata, then version metadata"). Worth keeping the issue open.

---

_Label `question` removed by @charliermarsh on 2024-09-24 15:28_

---

_Label `enhancement` added by @charliermarsh on 2024-09-24 15:28_

---
