---
number: 14754
title: "Unexpected: uv works offline initially, but fails after 5-10 minutes"
type: issue
state: closed
author: ajfriend
labels:
  - bug
assignees: []
created_at: 2025-07-20T17:03:57Z
updated_at: 2025-07-21T03:32:34Z
url: https://github.com/astral-sh/uv/issues/14754
synced_at: 2026-01-10T01:25:48Z
---

# Unexpected: uv works offline initially, but fails after 5-10 minutes

---

_Issue opened by @ajfriend on 2025-07-20 17:03_

### Summary

I'm new to `uv`, and so far it has been great. Thanks! But in true "If You Give a Mouse a Cookie" fashion, I've got an additional request :)

I'm often working without an internet connection, and it would be great if I could depend on `uv`'s package caching to build a project from scratch.

However, as I've played with it, it seems like this behavior only works for about 5--10 minutes after going offline, and fails afterwards with an error like:

```
Using CPython 3.13.5
Creating virtual environment at: .venv
⠸ uv-offline-test==0.1.0                                                                  error: Failed to fetch: `https://pypi.org/simple/pytest/`
  Caused by: Request failed after 3 retries
  Caused by: error sending request for url (https://pypi.org/simple/pytest/)
  Caused by: client error (Connect)
  Caused by: dns error
  Caused by: failed to lookup address information: nodename nor servname provided, or not known
```

I've got a minimal example repo with reproduction instructions at: https://github.com/ajfriend/uv_offline_test

(Normally, I might not delete the `.venv` or delete the lockfile, but it would be nice to not be afraid of doing these things, especially since they **do work** for about 10 minutes, currently.)

This behavior was surprising/unexpected to me since things worked initially offline, but failed later in the day. I had to do some experimentation to determine that the threshold was about 10 minutes. (In at least one test, it happened after 5 minutes, so it might be variable?)

I'm guessing this is due to some cache expiration policy? What's the reasoning behind the current 10 minutes, and would it be possible to extend it, or provide an option to do so? Would it make sense to defer/delay the cache expiration if `uv` notices that it is offline?

Thanks!

### Platform

Darwin 24.5.0 arm64

### Version

uv 0.8.0 (0b2357294 2025-07-17)

### Python version

Python 3.13.5

---

_Label `bug` added by @ajfriend on 2025-07-20 17:03_

---

_Comment by @zanieb on 2025-07-20 17:09_

10 minutes is the expiration time in PyPI cache headers. Have you used `--offline`? We won't revalidate requests then.

---

_Comment by @zanieb on 2025-07-20 17:10_

Related https://github.com/astral-sh/uv/issues/10380

---

_Comment by @ajfriend on 2025-07-20 18:43_

Thanks for the quick response!

Using `--offline` does mostly work for my use case, but I agree with the comment in https://github.com/astral-sh/uv/issues/10380: having a `--best-effort` or "Online with Fallback" mode would be ideal. Personally, I'd probably use such a flag as the default in my workflows, so I don’t have to change depending on whether I’m online or not.

I also understand the motivation to respect PyPI cache headers, but there feels like a bit of a discontinuity in the user experience: If I keep my existing environment, everything continues to work using stale cache. But if I try to recreate the exact same state from scratch (even using only previously-cached packages) I get penalized just because the metadata is "too old".

---

_Comment by @charliermarsh on 2025-07-21 01:47_

👍 Closing in favor of https://github.com/astral-sh/uv/issues/10380.

---

_Closed by @charliermarsh on 2025-07-21 01:47_

---

_Comment by @ajfriend on 2025-07-21 03:32_

👍 

For my future reference, the `UV_OFFLINE` environment variable is probably the easiest way to toggle this behavior.

E.g., justfile syntax:

```
export UV_OFFLINE := "1"
```

---
