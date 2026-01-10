```yaml
number: 4821
title: "add `dependencies` to --format JSON"
type: pull_request
state: closed
author: ChannyClaus
labels: []
assignees: []
base: main
head: pip-list-deps
created_at: 2024-07-04T21:58:06Z
updated_at: 2024-09-22T04:48:18Z
url: https://github.com/astral-sh/uv/pull/4821
synced_at: 2026-01-10T12:53:31Z
```

# add `dependencies` to --format JSON

---

_Pull request opened by @ChannyClaus on 2024-07-04 21:58_

## Summary
_maybe_ resolves https://github.com/astral-sh/uv/issues/4711?

wasn't sure if this is the preferred place to tack the `dependency` field on, but seemed like the least amount of code / work involved (also wasn't sure if this should be gated behind a flag).

## Test Plan
modified the existing test to include `"dependencies"` field.


---

_Closed by @ChannyClaus on 2024-09-22 04:48_

---
