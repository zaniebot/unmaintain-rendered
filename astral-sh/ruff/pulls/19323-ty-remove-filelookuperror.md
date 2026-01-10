```yaml
number: 19323
title: "[ty] Remove `FileLookupError`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - server
  - ty
assignees: []
merged: true
base: main
head: dhruv/remove-file-lookup-error
created_at: 2025-07-14T08:45:15Z
updated_at: 2025-07-14T13:35:51Z
url: https://github.com/astral-sh/ruff/pull/19323
synced_at: 2026-01-10T18:33:12Z
```

# [ty] Remove `FileLookupError`

---

_Pull request opened by @dhruvmanila on 2025-07-14 08:45_

## Summary

This PR removes the `FileLookupError` as it's not really required. The original intention was that this would be returned from the `.file` lookup to the different handlers but we've since moved the logic of "lookup file and add trace message if file unavailable with the reason" under the `file_ok` method which all of the handlers use.


---

_Review requested from @carljm by @dhruvmanila on 2025-07-14 08:45_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-07-14 08:45_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-07-14 08:45_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-07-14 08:45_

---

_Review requested from @dcreager by @dhruvmanila on 2025-07-14 08:45_

---

_Label `internal` added by @dhruvmanila on 2025-07-14 08:45_

---

_Label `server` added by @dhruvmanila on 2025-07-14 08:45_

---

_Label `ty` added by @dhruvmanila on 2025-07-14 08:45_

---

_Review request for @dcreager removed by @dhruvmanila on 2025-07-14 08:45_

---

_Review request for @carljm removed by @dhruvmanila on 2025-07-14 08:45_

---

_Review request for @sharkdp removed by @dhruvmanila on 2025-07-14 08:45_

---

_Review request for @AlexWaygood removed by @dhruvmanila on 2025-07-14 08:45_

---

_Comment by @github-actions[bot] on 2025-07-14 08:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @MichaReiser on `crates/ty_server/src/session.rs`:467 on 2025-07-14 09:13_

Should we rename the function back to `file`, given that there's no `file() -> Result` method anymore?

---

_@MichaReiser approved on 2025-07-14 09:13_

---

_Merged by @dhruvmanila on 2025-07-14 13:35_

---

_Closed by @dhruvmanila on 2025-07-14 13:35_

---

_Branch deleted on 2025-07-14 13:35_

---
