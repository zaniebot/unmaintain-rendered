---
number: 1799
title: "Warn when registry doesn't support range requests for wheels"
type: issue
state: closed
author: konstin
labels:
  - enhancement
  - error messages
assignees: []
created_at: 2024-02-21T09:29:51Z
updated_at: 2024-11-23T03:32:16Z
url: https://github.com/astral-sh/uv/issues/1799
synced_at: 2026-01-10T01:23:09Z
---

# Warn when registry doesn't support range requests for wheels

---

_Issue opened by @konstin on 2024-02-21 09:29_

[Range requests](https://developer.mozilla.org/en-US/docs/Web/HTTP/Range_requests) are very important for uv's performance, especially with large wheels. They are the difference between downloading entire wheels and just some slices in the bytes to KB range. They have been part of the HTTP/1.1 and are supported by all major webserver and blob storages. We should warn users if their registry is misconfigured and doesn't support range requests. Without a warning, it is otherwise inexplicable to users why uv is slower than advertised.

---

_Label `enhancement` added by @konstin on 2024-02-21 09:29_

---

_Label `error messages` added by @konstin on 2024-02-21 09:29_

---

_Comment by @charliermarsh on 2024-02-21 12:41_

I don’t think these should be user-facing, since users typically have no control over this.

---

_Comment by @konstin on 2024-02-21 12:44_

I expect they'll need to forward this to whoever is managing their server, but i tend towards making it user visible as otherwise nobody will see this is missing.

---

_Comment by @charliermarsh on 2024-02-21 12:45_

But like, Artifactory doesn’t support this at all. Making these user-visible means Artifactory users will see these on every invocation with no recourse.

---

_Comment by @MichaReiser on 2024-02-21 12:47_

It's a difficult one. Making it visible to users could motivate artifactory to add support for it but showing it on every invocation sounds annoying. I wonder if there are other ways we could achieve the same. Maybe with a --profile flag that tells you where uv spends time. 

---

_Comment by @Eh2406 on 2024-04-10 22:21_

Perhaps only display the message if the just run command spent more than 1 second downloading wheels from a registry that does not support range queries. Something like:

```
This took longer than expected because registry (url) does not support Http 1.1 range queries.
```

---

_Comment by @hmc-cs-mdrissi on 2024-04-11 01:24_

`more than 1 second downloading wheels `

So every time for large pytorch (or other ml/data ecosystem libraries) wheels on registries that don't support range requests? I'd still lean to call it noise that's not usually not actionable for most users.

edit: If verbose mode shows this that's fine. If some flag related to performance shows this that's also reasonable. On every request sounds like a lot of corporate indices are likely to be noisy and fun time trying to find registry owner/asking them to work on it.

edit 2: Aggressive/smart caching uv has is very nice on a non-first run regardless of index having range requests.

---

_Comment by @charliermarsh on 2024-11-23 03:32_

I think I'm gonna close this. We do show this in verbose logging, but I just don't love showing non-actionable user-facing warnings like this.

---

_Closed by @charliermarsh on 2024-11-23 03:32_

---
