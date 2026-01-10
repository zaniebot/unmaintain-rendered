```yaml
number: 18544
title: "[ty] Fix stale documents on Windows"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - windows
  - ty
assignees: []
merged: true
base: main
head: micha/ty-server-document-key-api
created_at: 2025-06-08T06:46:58Z
updated_at: 2025-06-09T14:39:13Z
url: https://github.com/astral-sh/ruff/pull/18544
synced_at: 2026-01-10T18:45:04Z
```

# [ty] Fix stale documents on Windows

---

_Pull request opened by @MichaReiser on 2025-06-08 06:46_

## Summary

This is mostly a refactor which should fix the stale diagnostics on Windows (I've to test this when I'm back home and have access to my windows machine).

The main refactor is that `Index` now stores a map from `AnySystemPath` to the documents instead of `Url`. This removes the need to convert between `Url -> AnySystemPath` and `AnySystemPath -> Url`, which can lead mismatches.

Closes https://github.com/astral-sh/ty/issues/601

## Test Plan

Tested that completions and diagnostics work on macos. 


https://github.com/user-attachments/assets/0560c56a-8fa1-44a7-a8b9-c1342d674e53




---

_Label `server` added by @MichaReiser on 2025-06-08 06:47_

---

_Label `ty` added by @MichaReiser on 2025-06-08 06:47_

---

_Comment by @github-actions[bot] on 2025-06-08 06:50_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @MichaReiser on 2025-06-08 07:45_

I still have to test if this fixes the bug on windows. I plan to do this later today. But I think this is good for review.

---

_Marked ready for review by @MichaReiser on 2025-06-08 07:45_

---

_Review requested from @carljm by @MichaReiser on 2025-06-08 07:46_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-06-08 07:46_

---

_Review requested from @sharkdp by @MichaReiser on 2025-06-08 07:46_

---

_Review requested from @dcreager by @MichaReiser on 2025-06-08 07:46_

---

_Review request for @dcreager removed by @MichaReiser on 2025-06-08 07:46_

---

_Review request for @carljm removed by @MichaReiser on 2025-06-08 07:46_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-06-08 07:46_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-06-08 07:46_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-06-08 07:46_

---

_Label `windows` added by @AlexWaygood on 2025-06-08 09:31_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/notifications/did_change.rs`:34 on 2025-06-09 06:01_

Unrelated to your PR, there are various instances of this kind of failures like this one and then there's `key.to_url()` where adding these debug logs consistently would be useful.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/document.rs`:52 on 2025-06-09 06:03_

Should we keep the `Url` as well for both `Notebook` and `Text` variants? That might help to avoid converting between `AnySystemPath` and `Url`. This would make the `key.to_url` calls to return `Url` instead of `Option<Url>` and avoid the cost of converting a path to `Url` when we already had the `Url` before.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/document.rs`:52 on 2025-06-09 06:06_

Oh, but the `Url` is not available in the `LSPSystem`.

---

_@dhruvmanila approved on 2025-06-09 08:24_

Looks good, thank you for jumping on and fixing this quickly!

---

_Comment by @dhruvmanila on 2025-06-09 08:25_

> I still have to test if this fixes the bug on windows. I plan to do this later today. But I think this is good for review.

I'm assuming that the video in the PR description is on Windows.

---

_@MichaReiser reviewed on 2025-06-09 14:38_

---

_Review comment by @MichaReiser on `crates/ty_server/src/document.rs`:52 on 2025-06-09 14:38_

Yes, and the client seems to be fine if the urls are slightly different. We could consider making AnySystemPath more strict so that toUrl could never fail but seems like a different refactor

---

_Merged by @MichaReiser on 2025-06-09 14:39_

---

_Closed by @MichaReiser on 2025-06-09 14:39_

---

_Branch deleted on 2025-06-09 14:39_

---
