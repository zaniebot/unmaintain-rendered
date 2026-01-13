```yaml
number: 22556
title: "FAST00[123] option for specifying custom router classes"
type: issue
state: open
author: foxfriends
labels:
  - configuration
assignees: []
created_at: 2026-01-13T17:30:04Z
updated_at: 2026-01-13T19:24:56Z
url: https://github.com/astral-sh/ruff/issues/22556
synced_at: 2026-01-13T19:37:17Z
```

# FAST00[123] option for specifying custom router classes

---

_@foxfriends_

### Summary

This was mentioned in #14179, so I won't go into too much of the same detail again, but I see the issue was closed without anything being implemented.

We use a wrapped version of the FastAPI router, with the same semantics but different implementation, and it would be nice for these rules to apply to our custom router.

An option to specify additional class paths to custom router classes would be sufficient.

---

_Comment by @ntBre on 2026-01-13 19:24_

Thanks for re-opening the discussion! I think it would be reasonable to add a configuration option here, analogous to `logger-objects` as Micha said.

---

_Label `configuration` added by @ntBre on 2026-01-13 19:24_

---
