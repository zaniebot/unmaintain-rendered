```yaml
number: 17467
title: Deprecate non-PEP 625 source distributions
type: pull_request
state: open
author: woodruffw
labels: []
assignees: []
draft: true
base: main
head: ww/deprecate-non-pep625
created_at: 2026-01-14T15:58:33Z
updated_at: 2026-01-14T16:20:53Z
url: https://github.com/astral-sh/uv/pull/17467
synced_at: 2026-01-14T16:39:18Z
```

# Deprecate non-PEP 625 source distributions

---

_@woodruffw_

## Summary

This adds a deprecation warning for source distributions that don't follow the PEP 625 filename scheme, which requires a `.tar.gz` suffix. I've also updated the docs with a warning about behavior + deprecation notice.

Towards #16911.

## Test Plan

The only functional change here is a user-level warning. I'll bump the snapshots that change.

---

_Assigned to @woodruffw by @woodruffw on 2026-01-14 15:58_

---

_Comment by @woodruffw on 2026-01-14 16:20_

Noting: I put this warning at the sdist filename parsing layer to start, but that's going to be *way* too noisy since it'll ding on every legacy sdist on PyPI (and there are a lot of them in the tail of popular projects). I think the better place to put this is in the installation planner, and limit it to just to-be-fetched remote dists. 

---
