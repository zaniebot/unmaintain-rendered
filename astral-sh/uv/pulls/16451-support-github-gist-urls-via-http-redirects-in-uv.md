```yaml
number: 16451
title: "Support GitHub Gist URLs via HTTP redirects in `uv run`"
type: pull_request
state: merged
author: twilligon
labels: []
assignees: []
merged: true
base: main
head: gist-redirect
created_at: 2025-10-26T00:20:57Z
updated_at: 2025-10-29T22:41:24Z
url: https://github.com/astral-sh/uv/pull/16451
synced_at: 2026-01-12T16:12:15Z
```

# Support GitHub Gist URLs via HTTP redirects in `uv run`

---

_@twilligon_

## Summary

Extend the existing GitHub Gist URL support from #15058 to handle URLs that redirect to Gists.

`reqwest` already handled generic URL redirects for us (note this redirects directly to the `.py` on `gist.githubusercontent.com`):

    ~/git/uv $ uv run https://httpbin.org/redirect-to?url=https://gist.githubusercontent.com/twilligon/4d878a4d9550a4f1df258cde1f058699/raw/c28a4bf0cb6bb9e670cb47c95d28971ffac163e5/hello.py
    hello world!

But running a URL that redirected to a Gist's "main page" (a bit.ly link leading to a Gist, etc.) did not:

    ~/git/uv $ uv run https://httpbin.org/redirect-to?url=https://gist.github.com/twilligon/4d878a4d9550a4f1df258cde1f058699
      File "/tmp/scriptNodt3Q.py", line 87
        <title>hello.py · GitHub</title>

But if we have `reqwest` follow redirects *before* `resolve_gist_url`, we can handle this fine:

    ~/git/uv $ target/debug/uv run https://httpbin.org/redirect-to?url=https://gist.github.com/twilligon/4d878a4d9550a4f1df258cde1f058699
    hello world!

## Test Plan

I'd write an automated test but that'd require network access since wiremock doesn't seem to support mocking specific hostnames like `gist.github.com`. As manual tests go, I basically did the above, testing with several redirectors to both generic and Gist URLs.

---

_Renamed from "Support GitHub Gist URLs via HTTP redirects in uv run" to "Support GitHub Gist URLs via HTTP redirects in `uv run`" by @twilligon on 2025-10-26 00:21_

---

_@charliermarsh approved on 2025-10-29 16:30_

That seems reasonable.

---

_Merged by @charliermarsh on 2025-10-29 16:30_

---

_Closed by @charliermarsh on 2025-10-29 16:30_

---

_Comment by @twilligon on 2025-10-29 22:41_

Thanks!

---
