```yaml
number: 16426
title: "[red-knot] Reject obvious `snapshot-diagnostics` typos in mdtests"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
draft: true
base: main
head: alex/snapshot-diagnostics-typos
created_at: 2025-02-27T22:53:21Z
updated_at: 2025-02-28T10:04:49Z
url: https://github.com/astral-sh/ruff/pull/16426
synced_at: 2026-01-12T15:55:54Z
```

# [red-knot] Reject obvious `snapshot-diagnostics` typos in mdtests

---

_@AlexWaygood_

## Summary

This PR adds some heuristics to the mdtest framework so that obvious typos such as `<!-- snpashot-diagnostics` or `<!-- snpshot-dignosstics -->` are rejected. The heuristics are that if the HTML comment is not equal to `snapshot-diagnostics`, but has a high fuzzy similarity score and is similar in length, we deduce that it's probably a typo.

I'm not sure at all this is the way to go, but I figured I'd put it up to see what people think of the idea. The pros of this PR are that it's an easy mistake to make to have a typo in a `snapshot-diagnostics` HTML comment, and that this PR doesn't add much complexity to mdtest. The cons are that we add some dissatisfying heuristics to mdtest, and add a new (admittedly very small) third-party dependency.

Note that the `looks_like_snapshot_directive_typo()` function introduced in this PR also has _some_ false positives. It rejects HTML comments such as `<!-- snapdragon-diagnostics -->` as being too similar to `<!-- snapshot-diagnostics -->`. It _does_ seem pretty unlikely that anybody is going to actually add a `<!-- snapdragon-diagnostics -->` HTML comment to an mdtest, however; we have the luxury of knowing that this framework is only for internal use.

Fixes https://github.com/astral-sh/ruff/issues/16352

## Test Plan

Added unit tests


---

_Label `testing` added by @AlexWaygood on 2025-02-27 22:53_

---

_Label `red-knot` added by @AlexWaygood on 2025-02-27 22:53_

---

_Review requested from @BurntSushi by @AlexWaygood on 2025-02-27 22:53_

---

_Review requested from @carljm by @AlexWaygood on 2025-02-27 22:53_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-02-27 22:53_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-02-27 22:53_

---

_Comment by @AlexWaygood on 2025-02-27 23:00_

aw c'mon, now the `typos` pre-commit hook is complaining about the test I added to make sure that we detected typos

---

_Comment by @github-actions[bot] on 2025-02-27 23:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @AlexWaygood on 2025-02-27 23:08_

> aw c'mon, now the `typos` pre-commit hook is complaining about the test I added to make sure that we detected typos

...and I suppose that does point to another obvious approach that avoids false positives, which is to do exactly what that project does (hard-code a list of obvious typos for `snapshot` and `diagnostics`, and reject HTML comments that contain any of them)

---

_Comment by @AlexWaygood on 2025-02-27 23:16_

I'm actually unsure why that pre-commit hook is catching the typo in `.rs` files but not in `.md` files

---

_@carljm approved on 2025-02-28 00:53_

I'm OK with this, but I think I'd prefer the simpler plan of just having an allowlist of HTML comments that are not an error in an mdtest. It looks to me like the only ones currently used are `snapshot-diagnostics`, `blacken-docs:on`, and `blacken-docs:off`. That's a very manageable allowlist, and I don't really see why we'd need freeform HTML comments in mdtests.

---

_Converted to draft by @AlexWaygood on 2025-02-28 00:58_

---

_Closed by @AlexWaygood on 2025-02-28 10:04_

---

_Branch deleted on 2025-02-28 10:04_

---
