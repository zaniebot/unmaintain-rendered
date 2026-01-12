```yaml
number: 13020
title: "[flake8-pyi] Skip Annotated and Literal for `string-or-bytes-too-long (PYI053)`"
type: pull_request
state: closed
author: dylwil3
labels:
  - bug
assignees: []
base: main
head: pyi053-redux
created_at: 2024-08-21T03:56:09Z
updated_at: 2024-12-05T21:23:43Z
url: https://github.com/astral-sh/ruff/pull/13020
synced_at: 2026-01-12T15:55:42Z
```

# [flake8-pyi] Skip Annotated and Literal for `string-or-bytes-too-long (PYI053)`

---

_@dylwil3_

PYI053 now skips strings which appear inside `Literal[...]` or `Annotated[...]`.

This is a modification of #13002, which skipped any type annotation for this rule. So, on the one hand, this PR allows the check to proceed in some cases that the prior PR skipped (e.g. return type annotations wrapped - unnecessarily, for a stub file - in quotes), and, on the other hand, this PR skips certain strings that the prior PR did not (e.g. type alias assignments to a `Literal` or `Annotated` type.)

In order to implement this change, the PR adds a new semantic flag for PEP593 annotations (i.e. `typing.Annotated[...]`) 

Closes #12995 (hopefully!)


---

_Review requested from @AlexWaygood by @dylwil3 on 2024-08-21 03:56_

---

_Label `bug` added by @dhruvmanila on 2024-08-21 04:00_

---

_Comment by @github-actions[bot] on 2024-08-21 04:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-08-21 11:28_

---

_@AlexWaygood reviewed on 2024-08-27 18:03_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/model.rs`:2246 on 2024-08-27 18:03_

oof... but I have been trying to think of a better name for 10 minutes and I've got nothing 😆

---

_Comment by @AlexWaygood on 2024-08-27 18:12_

I know this has been waiting on me for a little bit -- have to call it for the day now, but will try to land it tomorrow!

---

_Comment by @dylwil3 on 2024-08-27 18:30_

> I know this has been waiting on me for a little bit -- have to call it for the day now, but will try to land it tomorrow!

No worries at all!

---

_@dhruvmanila reviewed on 2024-08-28 05:40_

---

_Review comment by @dhruvmanila on `crates/ruff_python_semantic/src/model.rs`:2246 on 2024-08-28 05:40_

Seems like a reasonable name given other variants although I leave this up to you to decide, maybe you come up with a better name in the morning ;)

---

_@dhruvmanila reviewed on 2024-08-28 05:41_

---

_Review comment by @dhruvmanila on `crates/ruff_python_semantic/src/model.rs`:2246 on 2024-08-28 05:41_

Aside, it would be useful to provide a link to PEP 593 in the documentation above @dylwil3 

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-09-02 06:26_

---

_Comment by @dylwil3 on 2024-09-20 03:05_

@AlexWaygood if it was possible to meekly whisper a nudge in the form of a notification, I would, but github allows for only one volume. Let me know if there's anything else you'd like me to do on this one 😄 

---

_Comment by @charliermarsh on 2024-09-20 03:13_

@dylwil3 - Just a heads up (in case it wasn't mentioned elsewhere) that Alex is on vacation this week so may be a few more days before he gets to it. (It's totally fine that you nudged and in fact your nudge was very gracious!)

---

_Comment by @dylwil3 on 2024-12-05 21:23_

Closing in favor of #14797 

---

_Closed by @dylwil3 on 2024-12-05 21:23_

---
