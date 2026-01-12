```yaml
number: 15841
title: Add GHE support for gist resolution with GH_HOST
type: pull_request
state: open
author: harshithvh
labels: []
assignees: []
base: main
head: hvh/gh-host-support
created_at: 2025-09-14T15:51:51Z
updated_at: 2025-10-31T11:13:30Z
url: https://github.com/astral-sh/uv/pull/15841
synced_at: 2026-01-12T16:11:58Z
```

# Add GHE support for gist resolution with GH_HOST

---

_@harshithvh_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

- **GH_HOST environment variable**
- **URL-based host detection**
- **GitHub CLI-compatible API patterns** using `/api/v3/` endpoints
- **Dual pattern support**: subdomain (`gist.enterprise.com`) and path (`enterprise.com/gist/`)

Thanks for this (https://github.com/astral-sh/uv/issues/15109#issuecomment-3211400349) @jorgehermo9 🙏 

closes #15109

## Test Plan

- URL-based vs environment-based detection scenarios
- integration tests for all Enterprise patterns


---

_Comment by @jorgehermo9 on 2025-09-14 18:21_

As I commented in https://github.com/astral-sh/uv/issues/15109#issuecomment-3211400349 I think we shouldn't use the `GH_HOST` env var unless we add something like `uv run --gist <gist_id>`, because with what we have right now, we already know the host we should use, as the input is the gist's web url and not only the id. Allowing the `GH_HOST` would be pointless in my opinion (at least right now)

---

_Comment by @harshithvh on 2025-09-14 18:35_

> As I commented in [#15109 (comment)](https://github.com/astral-sh/uv/issues/15109#issuecomment-3211400349) I think we shouldn't use the `GH_HOST` env var unless we add something like `uv run --gist <gist_id>`, because with what we have right now, we already know the host we should use, as the input is the gist's web url and not only the id. Allowing the `GH_HOST` would be pointless in my opinion (at least right now)

Hm, I don't see the harm/downside in keeping GH_HOST support, when we add `uv run --gist <gist_id>` enterprise users will need GH_HOST. Removing it now = breaking change later.

---

_Comment by @harshithvh on 2025-10-31 11:13_

@zanieb , it would be great if you could review this when you have some time
Thanks!

---
