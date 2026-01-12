```yaml
number: 19736
title: "[ty] Link directly to typing conformance test suite when commenting the diff"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: alex/typing-conformance-diff
created_at: 2025-08-04T09:33:39Z
updated_at: 2025-08-04T18:24:50Z
url: https://github.com/astral-sh/ruff/pull/19736
synced_at: 2026-01-12T15:56:45Z
```

# [ty] Link directly to typing conformance test suite when commenting the diff

---

_@AlexWaygood_

## Summary

A small quality-of-life improvement to make it easier to evaluate whether a typing-conformance diff is good or bad

## Test Plan

<!-- How was it tested? -->


---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-04 09:33_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2025-08-04 09:33_

---

_Label `ci` added by @AlexWaygood on 2025-08-04 09:33_

---

_Label `ty` added by @AlexWaygood on 2025-08-04 09:33_

---

_Renamed from "Link directly to typing conformance test suite when commenting the diff" to "[ty] Link directly to typing conformance test suite when commenting the diff" by @AlexWaygood on 2025-08-04 09:34_

---

_Comment by @github-actions[bot] on 2025-08-04 09:35_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Review comment by @dhruvmanila on `.github/workflows/typing_conformance_comment.yaml`:64 on 2025-08-04 09:38_

The workflow uses a specific commit which should be used here instead of `main`:

https://github.com/astral-sh/ruff/blob/808c94d509e1e3f3d22218926d08c98381ede7eb/.github/workflows/typing_conformance.yaml#L43-L43

---

_@dhruvmanila approved on 2025-08-04 09:38_

Thank you!

---

_@AlexWaygood reviewed on 2025-08-04 09:39_

---

_Review comment by @AlexWaygood on `.github/workflows/typing_conformance_comment.yaml`:64 on 2025-08-04 09:39_

Ahh, I should have checked, sorry!

---

_@dhruvmanila reviewed on 2025-08-04 09:54_

---

_Review comment by @dhruvmanila on `.github/workflows/typing_conformance_comment.yaml`:64 on 2025-08-04 09:54_

No problem!

---

_Review requested from @dhruvmanila by @AlexWaygood on 2025-08-04 09:58_

---

_Review comment by @dhruvmanila on `.github/workflows/typing_conformance_comment.yaml`:80 on 2025-08-04 14:39_

nit: it might be worth linking directly to the `conformance/tests` directory?

---

_@dhruvmanila approved on 2025-08-04 14:39_

---

_@AlexWaygood reviewed on 2025-08-04 14:51_

---

_Review comment by @AlexWaygood on `.github/workflows/typing_conformance_comment.yaml`:80 on 2025-08-04 14:51_

I considered it, but I like that this directory has a `README.md` file (which might be useful for first-time contributors!), whereas the `tests` subdirectory doesn't :-)

---

_Merged by @AlexWaygood on 2025-08-04 14:51_

---

_Closed by @AlexWaygood on 2025-08-04 14:51_

---

_Branch deleted on 2025-08-04 14:51_

---

_Comment by @AlexWaygood on 2025-08-04 18:24_

Ugh, it appears to be uploading the commit correctly: https://github.com/astral-sh/ruff/actions/runs/16730858154?pr=19669

And I _think_ it is downloading it correctly: https://github.com/astral-sh/ruff/actions/runs/16730894339/job/47358456147

but the links are not making it into the comment correctly, for whatever reason: https://github.com/astral-sh/ruff/pull/19669#issuecomment-3141064137

---
