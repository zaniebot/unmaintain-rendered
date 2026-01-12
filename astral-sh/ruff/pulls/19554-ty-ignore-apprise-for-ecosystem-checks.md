```yaml
number: 19554
title: "[ty] Ignore 'apprise' for ecosystem checks"
type: pull_request
state: closed
author: sharkdp
labels:
  - internal
  - testing
  - ty
assignees: []
base: main
head: david/skip-running-on-apprise
created_at: 2025-07-25T11:42:10Z
updated_at: 2025-07-25T20:49:33Z
url: https://github.com/astral-sh/ruff/pull/19554
synced_at: 2026-01-12T15:56:42Z
```

# [ty] Ignore 'apprise' for ecosystem checks

---

_@sharkdp_

## Summary

The `apprise` project has outdated (and completely wrong) stub files that frequently lead to very confusing errors (usually true positives). Evaluating ecosystem impact on this project is therefore irritating and usually not very helpful. I've wasted a lot of time looking through ecosystem diffs on this project (until I remember — oh, it's "apprise"), and I don't think that it was ever helpful. I therefore suggest to skip evaluating on `apprise`. If there are no objections, we can also think about removing that project upstream (in `mypy_primer`).

[1] https://github.com/caronc/apprise/issues/1211
[2] https://github.com/caronc/apprise/blob/91faed0c6d22b49e7f31bc9dec1c8742f52a0aa3/apprise/config/base.pyi



---

_Review requested from @carljm by @sharkdp on 2025-07-25 11:42_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-25 11:42_

---

_Review requested from @dcreager by @sharkdp on 2025-07-25 11:42_

---

_Label `internal` added by @sharkdp on 2025-07-25 11:42_

---

_Label `testing` added by @sharkdp on 2025-07-25 11:42_

---

_Label `ty` added by @sharkdp on 2025-07-25 11:42_

---

_Renamed from "[ty] Ignoring 'apprise' for ecosystem checks" to "[ty] Ignore 'apprise' for ecosystem checks" by @sharkdp on 2025-07-25 11:42_

---

_Comment by @AlexWaygood on 2025-07-25 11:44_

If memory serves correctly, this project was originally added to primer because mypy introduced a regression in one release that meant that it started crashing on any code that imported `apprise`. So the strangeness of the code has some advantages as well as disadvantages: it helps test edge cases that we might not have thought of, because they just don't occur in "well-written" code.

Would it be possible to continue running it as part of mypy_primer (so that we catch any crashes on the project we might introduce in ty) but filter it out of the diff report?

---

_Comment by @github-actions[bot] on 2025-07-25 11:46_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @sharkdp on 2025-07-25 11:47_

> meant that it started crashing on any code that imported `apprise`. So the strangeness of the code has some advantages as well as disadvantages: it helps test edge cases that we

That's certainly possible, yes. We would probably have to start a second `mypy_primer` process in the CI job.

---

_Comment by @carljm on 2025-07-25 19:21_

I guess the other alternative is that we keep it and just keep in mind that it might not be worth digging into its results.

I don't have strong feelings.

---

_@carljm approved on 2025-07-25 19:21_

---

_Closed by @sharkdp on 2025-07-25 20:49_

---
