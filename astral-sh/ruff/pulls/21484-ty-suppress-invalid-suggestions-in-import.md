```yaml
number: 21484
title: "[ty] suppress invalid suggestions in import statements"
type: pull_request
state: merged
author: RasmusNygren
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: fix-import-alias-suggestions
created_at: 2025-11-16T20:11:25Z
updated_at: 2026-01-15T18:26:59Z
url: https://github.com/astral-sh/ruff/pull/21484
synced_at: 2026-01-15T18:49:33Z
```

# [ty] suppress invalid suggestions in import statements

---

_@RasmusNygren_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes https://github.com/astral-sh/ty/issues/1562

Only suggest the keyword "as" in import statements when the user have written `import foo a<CURSOR>` or `from foo import bar a<CURSOR>` as no other suggestion makes sense here.

Re-uses the existing pattern for incomplete `import from` statements to determine incomplete import alias statements and make the suggestions more sane in those cases.

There was a potential suggestion from @BurntSushi in https://github.com/astral-sh/ty/issues/1562#issue-3626853513 to move the handling of import statements into one unified state machine but I acted on the side of caution and fixed this with already established patterns, pending a potential bigger re-write down the line. 

## Test Plan

Added new tests and checked that it behaved reasonable in the playground.

<!-- How was it tested? -->


---

_Review requested from @carljm by @RasmusNygren on 2025-11-16 20:11_

---

_Review requested from @MichaReiser by @RasmusNygren on 2025-11-16 20:11_

---

_Review requested from @AlexWaygood by @RasmusNygren on 2025-11-16 20:11_

---

_Review requested from @sharkdp by @RasmusNygren on 2025-11-16 20:11_

---

_Review requested from @dcreager by @RasmusNygren on 2025-11-16 20:11_

---

_Label `server` added by @AlexWaygood on 2025-11-16 20:51_

---

_Label `ty` added by @AlexWaygood on 2025-11-16 20:51_

---

_Comment by @astral-sh-bot[bot] on 2025-11-16 21:18_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-16 21:20_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Review requested from @BurntSushi by @AlexWaygood on 2025-11-16 23:04_

---

_Review request for @dcreager removed by @MichaReiser on 2025-11-17 07:22_

---

_Review request for @carljm removed by @MichaReiser on 2025-11-17 07:22_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-11-17 07:22_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-11-17 07:22_

---

_@BurntSushi approved on 2025-11-17 14:02_

Thanks! I pushed some small fixups to the comments/names, but this otherwise LGTM. Nice tests! :)

---

_Merged by @BurntSushi on 2025-11-17 14:58_

---

_Closed by @BurntSushi on 2025-11-17 14:58_

---

_Branch deleted on 2026-01-15 18:26_

---
