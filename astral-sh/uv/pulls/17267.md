```yaml
number: 17267
title: Fix nested retry loops for transient network errors
type: pull_request
state: closed
author: shayonj
labels: []
assignees: []
draft: true
base: main
head: s/retry-fix
created_at: 2025-12-30T14:50:30Z
updated_at: 2025-12-30T18:45:19Z
url: https://github.com/astral-sh/uv/pull/17267
synced_at: 2026-01-10T05:49:14Z
```

# Fix nested retry loops for transient network errors

---

_Pull request opened by @shayonj on 2025-12-30 14:50_

## Summary

Fixes a regression where transient network errors (like DNS failures) cause nested retry loops, 
resulting in ~16 retry attempts instead of 4 and significantly longer failure times (~17-19s vs ~4s).

## Problem

After #17104, both the middleware (`RetryTransientMiddleware`) and the cached client layer 
(`RetryState::should_retry`) independently retry transient network errors. When the middleware 
exhausts its retry budget and returns an error, `should_retry` starts a fresh retry loop, 
causing nested retries.

## Fix

`RetryState::should_retry` now only tracks middleware retries for error reporting instead of 
initiating additional retries for transient errors. The middleware already handles transient 
retry logic, so duplicating it here causes the nested loops.

## Test Plan

- All existing network tests pass
- Manual test with invalid DNS shows ~4s failure time (was ~17-19s) using the replication script in https://github.com/astral-sh/uv/issues/17266


---

_Review requested from @konstin by @zanieb on 2025-12-30 15:11_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:1149 on 2025-12-30 15:25_

It's fine to break API compatibility here, every release is considered breaking at this time.

---

_@zanieb reviewed on 2025-12-30 15:25_

---

_Comment by @shayonj on 2025-12-30 18:13_

looks like there is a fix proposed in https://github.com/astral-sh/uv/pull/17274. Closing on behalf

---

_Closed by @shayonj on 2025-12-30 18:13_

---

_Branch deleted on 2025-12-30 18:13_

---

_Comment by @EliteTK on 2025-12-30 18:35_

@shayonj ah, my apologies, I had missed that you were already working on a fix.

---

_Comment by @shayonj on 2025-12-30 18:45_

no probs at all! thank you for the fast fix

---
