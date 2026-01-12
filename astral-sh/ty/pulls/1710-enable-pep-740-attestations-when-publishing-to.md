```yaml
number: 1710
title: Enable PEP 740 attestations when publishing to PyPI
type: pull_request
state: open
author: woodruffw
labels: []
assignees: []
draft: true
base: main
head: ww/pep740
created_at: 2025-12-01T18:14:24Z
updated_at: 2025-12-01T18:19:51Z
url: https://github.com/astral-sh/ty/pull/1710
synced_at: 2026-01-12T15:54:27Z
```

# Enable PEP 740 attestations when publishing to PyPI

---

_@woodruffw_

## Summary

This enables PEP 740 attestations on the distributions we publish to PyPI.

See https://github.com/astral-sh/uv/pull/16910 and https://github.com/astral-sh/ruff/pull/21735 for reference 🙂 

## Test Plan

Not easy to test, since it's in the release pipeline. The action itself has integration tests and has been confirmed to work on other project releases, however, so hopefully things will go smoothly here...

---

_Comment by @zanieb on 2025-12-01 18:16_

Yo let's wait until it's proven working before we break all three repositories :D

---

_Comment by @woodruffw on 2025-12-01 18:19_

> Yo let's wait until it's proven working before we break all three repositories :D

Wise words, I'll mark this one as drafted until we have a successful release on uv/ruff 😅 

---

_Converted to draft by @woodruffw on 2025-12-01 18:19_

---
