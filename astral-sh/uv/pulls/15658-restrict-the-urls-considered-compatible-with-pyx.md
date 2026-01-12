```yaml
number: 15658
title: Restrict the URLs considered compatible with pyx requests
type: pull_request
state: open
author: zanieb
labels: []
assignees: []
draft: true
base: main
head: zb/strict-known
created_at: 2025-09-03T12:44:08Z
updated_at: 2025-09-03T12:55:47Z
url: https://github.com/astral-sh/uv/pull/15658
synced_at: 2026-01-12T16:11:52Z
```

# Restrict the URLs considered compatible with pyx requests

---

_@zanieb_

The implementation is more awkward than before here because I'm trying to keep using the `Realm` type for comparisons. We may want to consider not doing that, but I figured I'd share the draft.

In short, this no longer accepts:

- A URL on a different scheme than the API (e.g., http vs https)
- A URL with a superdomain matching the API superdomain
- A URL with a port that differs from the API

We require the CDN scheme and port to match the API URL.

However, we allow some special cases:

- A URL with a domain matching the API with `api.` stripped.  
- A URL with an arbitrary subdomain of the CDN domain
- A CDN URL with the default port for the scheme (when the API is on a different port)

I've chosen to restrict the subdomains we'll treat as matching for the API because we _generally_ should not allow credentials to be propagated across subdomains. This becomes relevant if, e.g., a user is customizing the API URL to a subdomain like `pyx.my-services.foo`, where we should not bleed credentials to arbitrary other services.


---
