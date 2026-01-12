```yaml
number: 10075
title: Add additional explanation of --mount=type=cache
type: pull_request
state: open
author: mirober
labels: []
assignees: []
base: main
head: mr/cache-mount-explanation
created_at: 2024-12-21T12:19:49Z
updated_at: 2025-04-14T08:00:31Z
url: https://github.com/astral-sh/uv/pull/10075
synced_at: 2026-01-12T16:09:06Z
```

# Add additional explanation of --mount=type=cache

---

_@mirober_

## Summary

I had not seen this feature of docker before and it took a few minutes of reading the docker docs to clarify what it was doing. Hopefully this update makes it clear why a user would want to do this without duplicating the docker docs too much.

I also fixed the link to the docker docs, which was directing to the top of the page since the name of the anchor had changed.

---
