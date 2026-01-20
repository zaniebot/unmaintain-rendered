```yaml
number: 17631
title: "PEP 792: plumb statuses into internal representation"
type: pull_request
state: open
author: woodruffw
labels:
  - internal
assignees: []
draft: true
base: main
head: ww/pep792-plumbing
created_at: 2026-01-20T19:48:55Z
updated_at: 2026-01-20T19:48:55Z
url: https://github.com/astral-sh/uv/pull/17631
synced_at: 2026-01-20T20:55:56Z
```

# PEP 792: plumb statuses into internal representation

---

_@woodruffw_

## Summary

The idea here is to build atop #17311. Specifically, I'm taking the PEP 792 status markers pulled from each "index representation" and putting it into the `SimpleDetailMetadata` representation, which I understand to be the shared internal representation for detail responses.

## Test Plan

Will add unit tests for type changes.

---

_Assigned to @woodruffw by @woodruffw on 2026-01-20 19:48_

---

_Label `internal` added by @woodruffw on 2026-01-20 19:48_

---
