---
number: 15073
title: "Rate limited `uv self update` message loses detail for downloading specific version"
type: issue
state: open
author: nathanjmcdougall
labels:
  - bug
assignees: []
created_at: 2025-08-05T01:13:28Z
updated_at: 2025-11-04T09:38:27Z
url: https://github.com/astral-sh/uv/issues/15073
synced_at: 2026-01-07T13:12:19-06:00
---

# Rate limited `uv self update` message loses detail for downloading specific version

---

_Issue opened by @nathanjmcdougall on 2025-08-05 01:13_

### Summary

When hitting the GitHub rate limit, `uv self update` provides this helpful message:

```Powershell
> uv self update
info: Checking for updates...
error: GitHub API rate limit exceeded. Please provide a GitHub token via the `--token` option.
```

However, the same message is not given when requesting a specific version; instead the message is fairly cryptic:

```Powershell
PS C:\Users\namc\Repositories\ruru> uv self update 0.7.13
info: Checking for updates...
error: The version 0.7.13 was not found for the app uv in workspace uv
```

### Platform

Windows 10 Enterprise x86_64

### Version

uv 0.8.4 (e176e1714 2025-07-30)

### Python version

Python 3.12.11

---

_Label `bug` added by @nathanjmcdougall on 2025-08-05 01:13_

---

_Comment by @zanieb on 2025-08-05 01:47_

Huh, weird. I presume this is an axoupdater bug.

The error is defined at https://github.com/axodotdev/axoupdater/blob/73ef9c59d3d9bc728c80d085633ed26474b5aeb4/axoupdater/src/errors.rs#L135-L144

and presumably raised at https://github.com/axodotdev/axoupdater/blob/73ef9c59d3d9bc728c80d085633ed26474b5aeb4/axoupdater/src/release/github.rs#L150-L169

I'm not sure why it's an empty set of releases though? I'd expect the HTTP response to be raised.

---

_Comment by @runfalk on 2025-11-04 09:37_

Just ran into the same issue on `0.9.5` when trying to downgrade. This is mostly a problem on my work laptop as it uses Cloudflare WARP, i.e. I'm always rate limited due to shared IP-address. Worked around with a PAT but ideally the error message could be improved.

---
