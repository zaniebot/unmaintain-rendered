```yaml
number: 12330
title: "Fix cache_prune::prune_unzipped test to match updated error message format"
type: pull_request
state: merged
author: chhoumann
labels: []
assignees: []
merged: true
base: main
head: fix-cache-prune-test-only
created_at: 2025-03-20T07:39:31Z
updated_at: 2025-03-20T13:54:08Z
url: https://github.com/astral-sh/uv/pull/12330
synced_at: 2026-01-12T16:10:13Z
```

# Fix cache_prune::prune_unzipped test to match updated error message format

---

_@chhoumann_

## Summary

Fixes the failing `cache_prune::prune_unzipped` test that was causing CI failures in my other PR (#12328) and others like PR #12327.

The error message format changed to show a specific version constraint (`iniconfig<=2.0.0`) rather than the generic 'all versions' message. This PR updates the test to expect the new, more specific error message.

## Test Plan
Ran `cargo test -p uv cache_prune::prune_unzipped` to verify the test now passes.

---

_Merged by @charliermarsh on 2025-03-20 13:49_

---

_Closed by @charliermarsh on 2025-03-20 13:49_

---

_Comment by @charliermarsh on 2025-03-20 13:49_

Thanks. I think this is because the "available versions" list doesn't omit versions based on `--exclude-newer`...

---

_Branch deleted on 2025-03-20 13:54_

---
