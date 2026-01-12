```yaml
number: 12727
title: Fix cache invalidation for nested pyproject.toml files
type: pull_request
state: merged
author: MichaReiser
labels:
  - cli
assignees: []
merged: true
base: main
head: fix-12721
created_at: 2024-08-07T06:33:37Z
updated_at: 2024-08-07T19:53:48Z
url: https://github.com/astral-sh/ruff/pull/12727
synced_at: 2026-01-12T15:55:42Z
```

# Fix cache invalidation for nested pyproject.toml files

---

_@MichaReiser_

## Summary

This PR fixes an issue with cache invalidation where changing a nested pyproject.toml didn't invalidate the cache for those files. 

The root cause of the issue was that the `Resolver` returned the wrong configuration when querying the settings by the directory because it only registers a route for `directory/{files}` but a query by directory misses the trailing `/`. 

Fixes #12721 
Fixes #12264

## Test Plan

Played throught the example in #12721


---

_Review requested from @ibraheemdev by @MichaReiser on 2024-08-07 06:33_

---

_Label `cli` added by @MichaReiser on 2024-08-07 06:33_

---

_@MichaReiser reviewed on 2024-08-07 06:35_

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/resolver.rs`:168 on 2024-08-07 06:35_

@ibraheemdev is there some wild card expression that we could use that I'm not aware of instead of inserting two routes (e.g. `{path}(/{{#filepath}})?`)

---

_Comment by @github-actions[bot] on 2024-08-07 06:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @fellhorn on 2024-08-07 07:51_

Thank you, Micha! I can confirm it fixes both the test script from #12721 as well as our own original case, where we discovered the bug.

You folks are so fast! The issue is not even 12h old and you already have a fix :)

---

_@ibraheemdev reviewed on 2024-08-07 19:27_

---

_Review comment by @ibraheemdev on `crates/ruff_workspace/src/resolver.rs`:168 on 2024-08-07 19:27_

No, this is the recommended way of doing it.

---

_@ibraheemdev approved on 2024-08-07 19:27_

---

_Merged by @MichaReiser on 2024-08-07 19:53_

---

_Closed by @MichaReiser on 2024-08-07 19:53_

---

_Branch deleted on 2024-08-07 19:53_

---
