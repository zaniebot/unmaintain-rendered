```yaml
number: 17211
title: Add background autoprune to remove stale cache entries
type: pull_request
state: open
author: TrevorBurnham
labels:
  - enhancement
assignees: []
base: main
head: feature/cache-autoprune
created_at: 2025-12-21T22:31:10Z
updated_at: 2026-01-06T23:05:03Z
url: https://github.com/astral-sh/uv/pull/17211
synced_at: 2026-01-10T05:49:14Z
```

# Add background autoprune to remove stale cache entries

---

_Pull request opened by @TrevorBurnham on 2025-12-21 22:31_

Fixes #5731

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR implements automatic cache cleanup that runs in the background during normal
uv operations. This addresses issue #5731 by preventing unbounded cache growth
without requiring user intervention.

The autoprune task:
- Spawns as a fire-and-forget background task when the cache is initialized
- Removes unreferenced archives older than 1 hour (grace period for in-flight ops)
- Requires no exclusive lock since archives are immutable and uniquely identified;
  if there's no symlink referencing an archive, no process can be using it

## Test Plan

I've added unit tests for this feature. Having said that, I'm new to this project, so I'd appreciate any suggestions for testing more thoroughly.


---

_Closed by @TrevorBurnham on 2025-12-21 23:00_

---

_Reopened by @TrevorBurnham on 2025-12-21 23:14_

---

_Comment by @konstin on 2026-01-06 19:02_

Do I understand it correctly that this doesn't remove old unpacked wheels generally, just archives having no symlinks pointing to them?

I do think that needs to lock, there can be multiple uv versions running on a system and it's generally safer to lock than to use time based approaches.

---

_Label `enhancement` added by @konstin on 2026-01-06 19:02_

---

_Comment by @TrevorBurnham on 2026-01-06 22:57_

@konstin That's a very good catch! I've updated this PR to take the safer approach of only removing archives that are unreferenced.

---
