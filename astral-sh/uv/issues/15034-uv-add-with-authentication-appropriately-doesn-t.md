---
number: 15034
title: "uv add with authentication appropriately doesn't store the secret, but should store the username"
type: issue
state: open
author: mshunshin
labels:
  - bug
assignees: []
created_at: 2025-08-02T20:37:47Z
updated_at: 2025-08-02T20:37:47Z
url: https://github.com/astral-sh/uv/issues/15034
synced_at: 2026-01-10T01:25:51Z
---

# uv add with authentication appropriately doesn't store the secret, but should store the username

---

_Issue opened by @mshunshin on 2025-08-02 20:37_

### Summary

I have a lot of python projects on github.

Within my .netrc I have many entries such as:
```text
machine github.com
login reponame-read-only
password github_pat_######
```

When I do uv add
uv add "reponame @ git+https://reponame-read-only@github.com/mshunshin/reponame.git"

I end up with 

[tool.uv.sources]
reponame = { git = "https://github.com/mshunshin/reponame.git" }

when I should have:

[tool.uv.sources]
reponame = { git = "https://reponame-read-only@github.com/mshunshin/reponame.git" }

I can manually edit it, but it is an extra step for me to give users to be able to pull my code.

if the username isn't stored the correct login can't be used.

### Platform

macOS

### Version

uv 0.8.4 (e176e1714 2025-07-30)

### Python version

Python 3.11.13

---

_Label `bug` added by @mshunshin on 2025-08-02 20:37_

---
