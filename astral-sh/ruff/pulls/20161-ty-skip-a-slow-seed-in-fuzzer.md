```yaml
number: 20161
title: "[ty] skip a slow seed in fuzzer"
type: pull_request
state: merged
author: carljm
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: cjm/fuzzer
created_at: 2025-08-29T20:05:32Z
updated_at: 2025-08-29T23:16:36Z
url: https://github.com/astral-sh/ruff/pull/20161
synced_at: 2026-01-12T15:56:55Z
```

# [ty] skip a slow seed in fuzzer

---

_@carljm_

## Summary

Fuzzer seed 208 seems to be timing out all fuzzer runs on PRs today. This has happened on multiple unrelated PRs, as well as on an initial version of this PR that made a comment-only change in ty and didn't skip any seeds, so the timeout appears to be consistent in CI, on ty main branch, as of today, but it started happening due to some change in a factor outside ty; not sure what.

I checked the code generated for seed 208 locally, and it takes about 30s to check on current ty main branch. This is slow for a fuzzer seed, but shouldn't be slow enough to make it time out after 20min in CI (even accounting for GH runners being slower than my laptop.)

I tried to bisect the slowness of checking that code locally, but I didn't go back far enough to find the change that made it slow. In fact it seems like it became significantly faster in the last few days (on an older checkout I had to stop it after several minutes.) So whatever the cause of the slowness, it's not a recent change in ty.

I don't want to rabbit-hole on this right now (fuzzer-discovered issues are lower-priority than real-world-code issues), and need a working CI, so skip this seed for now until we can investigate it.

## Test Plan

CI. This PR contains a no-op (comment) change in ty, so that the fuzz test is triggered in CI and we can verify it now works (as well as verify, on the previous commit, that the fuzzer job is timing out on that seed, even with just a no-op change in ty.)


---

_Label `internal` added by @carljm on 2025-08-29 20:05_

---

_Label `ty` added by @carljm on 2025-08-29 20:05_

---

_Comment by @github-actions[bot] on 2025-08-29 20:13_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-29 20:16_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-08-29 20:36_

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

_Marked ready for review by @carljm on 2025-08-29 20:50_

---

_Review requested from @AlexWaygood by @carljm on 2025-08-29 20:50_

---

_Review requested from @sharkdp by @carljm on 2025-08-29 20:50_

---

_Review requested from @dcreager by @carljm on 2025-08-29 20:50_

---

_Comment by @carljm on 2025-08-29 20:59_

Going ahead and landing this to unblock CI, post-merge review welcome.

---

_Merged by @carljm on 2025-08-29 20:59_

---

_Closed by @carljm on 2025-08-29 20:59_

---

_Branch deleted on 2025-08-29 20:59_

---

_Comment by @AlexWaygood on 2025-08-29 21:21_

Hmm there was a new release of pysource-minimize released an hour ago — could be related? Can't remember if we have the dependency locked somewhere or not. https://github.com/15r10nk/pysource-codegen/releases/tag/v0.7.1

---

_Comment by @carljm on 2025-08-29 21:26_

> Hmm there was a new release of pysource-minimize released an hour ago — could be related?

Does seem quite likely that's the cause. We [only have a lower bound](https://github.com/astral-sh/ruff/blob/main/python/py-fuzzer/pyproject.toml#L8) on our version of `pysource-minimize` in `pyproject.toml`. There is a `uv.lock` file there, but I'm not sure if `uvx --from` will actually use that or not; I suspect not? Using `--from` treats py-fuzzer as a dependency, not as the current project.

---

_Comment by @AlexWaygood on 2025-08-29 23:16_

Oh I see what you mean. Huh, what a pain; I guess that uv.lock file is essentially useless then? Maybe we should just use --exclude-newer or something in CI to ensure stability when running py-fuzzer

---
