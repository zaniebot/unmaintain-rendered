```yaml
number: 14034
title: Give non-existent files a durability of at least Medium
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/file-status-durability
created_at: 2024-11-01T08:52:59Z
updated_at: 2024-11-01T15:44:32Z
url: https://github.com/astral-sh/ruff/pull/14034
synced_at: 2026-01-10T20:59:37Z
```

# Give non-existent files a durability of at least Medium

---

_Pull request opened by @MichaReiser on 2024-11-01 08:52_

## Summary

Partially addresses https://github.com/astral-sh/ty/issues/242

The solution isn't ideal, but our problem is that the module resolves first queries if certain first-party files exist and only then resolves the standard library modules. The issue with that is that any query that depends on a resolved standard library module now depends on non-existing LOW durability files, negating the benefit of giving standard library files high durability. 

This PR changes the durability of the `File::status` to at least medium. This is still not perfect because it means that all standard library queries that depend on module resolution still have a MEDIUM durability instead of HIGH, but it's better than what we have today, and it should allow Salsa to take the "fast-incremental path" more often. 

This possibly helps with https://github.com/astral-sh/ruff/pull/14024

## Test Plan

See benchmarks


---

_Review requested from @carljm by @MichaReiser on 2024-11-01 08:52_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-01 08:52_

---

_Review requested from @sharkdp by @MichaReiser on 2024-11-01 08:52_

---

_Label `red-knot` added by @MichaReiser on 2024-11-01 08:56_

---

_Comment by @codspeed-hq[bot] on 2024-11-01 08:58_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha/file-status-durability)

### Merging astral-sh/ruff#14034 will **improve performances by 7.43%**

<sub>Comparing <code>micha/file-status-durability</code> (c485dce) with <code>main</code> (48fa839)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `micha/file-status-durability` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `red_knot_check_file[incremental]` | 4.6 ms | 4.3 ms | +7.43% |


---

_Comment by @github-actions[bot] on 2024-11-01 09:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2024-11-01 15:42_

So just to make sure I understand the effect here: module resolutions that only use typeshed files (but first have to verify that some first-party paths don't exist) will now be MEDIUM instead of LOW durability, meaning we won't have to re-resolve those paths on any change to a first-party file. Adding a previously-not-existent first-party file will count as a MEDIUM durability input change, and cause those queries to have to re-execute. Is that an accurate summary?

Looks good!

---

_Comment by @MichaReiser on 2024-11-01 15:43_

> So just to make sure I understand the effect here: module resolutions that only use typeshed files (but first have to verify that some first-party paths don't exist) will now be MEDIUM instead of LOW durability, meaning we won't have to re-resolve those paths on any change to a first-party file. Adding a previously-not-existent first-party file will count as a MEDIUM durability input change, and cause those queries to have to re-execute. Is that an accurate summary?
> 
> Looks good!

Yes, that's a perfect summary.

---

_Merged by @MichaReiser on 2024-11-01 15:44_

---

_Closed by @MichaReiser on 2024-11-01 15:44_

---

_Branch deleted on 2024-11-01 15:44_

---
