```yaml
number: 14632
title: " Refactor `crates/ruff_python_stdlib/src/builtins.rs` to make it easier to add support for new Python versions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: alex/314
created_at: 2024-11-27T10:48:18Z
updated_at: 2024-11-27T12:22:30Z
url: https://github.com/astral-sh/ruff/pull/14632
synced_at: 2026-01-12T15:55:48Z
```

#  Refactor `crates/ruff_python_stdlib/src/builtins.rs` to make it easier to add support for new Python versions

---

_@AlexWaygood_

## Summary

[Summary is now out of date, but is preserved for posterity. See conversation below.]

This PR adds Python 3.14 to the list of versions that we allow users to select for the `target-version` setting. The motivation is that this will allow us to detect if the target version is Python 3.14 or higher in #14623 and, if so, we can recommend using the faster function `Path.scandir()` in that rule rather than `Path.iterdir()`.

This PR _does not_ generally declare that Ruff _supports_ Python 3.14. I'm pretty hesitant to do that while Python 3.14 is still in its alpha period, since there might be many APIs (and even new syntax) added between now and the point when the first 3.14 beta is released. However, I don't see much harm in allowing users to _select_ Python 3.14 as their target version now, even if we don't yet officially declare that we support Python 3.14.

## Test Plan

`cargo test`


---

_Label `configuration` added by @AlexWaygood on 2024-11-27 10:48_

---

_Review requested from @carljm by @AlexWaygood on 2024-11-27 10:48_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-11-27 10:48_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-11-27 10:48_

---

_Comment by @AlexWaygood on 2024-11-27 10:49_

The alternative to doing this is that we just open an issue reminding us to update the rule being added in https://github.com/astral-sh/ruff/pull/14623 when we formally declare support for Python 3.14, after the first 3.14 beta is released. But I think it's pretty likely we'll just forget about that issue, and won't actually update the rule when the time comes.

---

_Converted to draft by @AlexWaygood on 2024-11-27 10:52_

---

_Marked ready for review by @AlexWaygood on 2024-11-27 11:08_

---

_Comment by @github-actions[bot] on 2024-11-27 11:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-11-27 11:50_

> I don't see much harm in allowing users to select Python 3.14 as their target version now, even if we don't yet officially declare that we support Python 3.14.

I don't disagree with this but I also don't see a significant benefit to it. I don't mind adding it to the list if we think adding https://github.com/astral-sh/ruff/pull/14623 now is the right choice but I would otherwise wait until we have an actual use case for it (or I'm worried that we'll get bug reports for Python 3.14 features)

---

_Renamed from "Add Python 3.14 to the list of allowed Python versions" to " Refactor `crates/ruff_python_stdlib/src/builtins.rs` to make it easier to add support for new Python versions" by @AlexWaygood on 2024-11-27 12:16_

---

_Review request for @carljm removed by @AlexWaygood on 2024-11-27 12:16_

---

_Review request for @MichaReiser removed by @AlexWaygood on 2024-11-27 12:16_

---

_Review request for @sharkdp removed by @AlexWaygood on 2024-11-27 12:16_

---

_Comment by @AlexWaygood on 2024-11-27 12:18_

https://github.com/astral-sh/ruff/pull/14623 was rejected, so there's no longer a strong reason to add `Py314` variants to the various enums we have around the repo. I don't think it's awful to allow users to select it before we officially support it, but it's at least a bit weird, so I've abandoned that for now. All that's left are some minor refactors that should make adding support for Python 3.14 a bit easier when the time comes to do so.

---

_Label `configuration` removed by @AlexWaygood on 2024-11-27 12:18_

---

_Label `internal` added by @AlexWaygood on 2024-11-27 12:18_

---

_Merged by @AlexWaygood on 2024-11-27 12:20_

---

_Closed by @AlexWaygood on 2024-11-27 12:20_

---

_Branch deleted on 2024-11-27 12:20_

---
