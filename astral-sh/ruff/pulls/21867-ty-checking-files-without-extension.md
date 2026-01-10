```yaml
number: 21867
title: "[ty] Checking files without extension"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/check-files-without-extensions
created_at: 2025-12-09T16:16:16Z
updated_at: 2025-12-10T16:47:43Z
url: https://github.com/astral-sh/ruff/pull/21867
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Checking files without extension

---

_Pull request opened by @MichaReiser on 2025-12-09 16:16_

## Summary

Adds support for checking files without extensions if they're passed by name on the CLI or 
if the user selects `Python` as the language in their editor. 

Fixes https://github.com/astral-sh/ty/issues/1692

Note: We don't yet support excluding a file that has a `.py` extension
if the user selects a non-Python language in their editor but I consider this a separate change.

## Test Plan

Added end to end tests


---

_Review requested from @carljm by @MichaReiser on 2025-12-09 16:16_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-09 16:16_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-09 16:16_

---

_Label `ty` added by @MichaReiser on 2025-12-09 16:16_

---

_Comment by @astral-sh-bot[bot] on 2025-12-09 16:18_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Review request for @dcreager removed by @MichaReiser on 2025-12-09 16:19_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-12-09 16:19_

---

_Review requested from @Gankra by @MichaReiser on 2025-12-09 16:19_

---

_Review comment by @Gankra on `crates/ty_server/src/session.rs`:1571 on 2025-12-09 16:21_

Hmm this check seems kinda adhoc/brittle

---

_Comment by @astral-sh-bot[bot] on 2025-12-09 16:22_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5258 diagnostics
+ Found 5259 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@Gankra approved on 2025-12-09 16:24_

Ah good, this is roughly what I was expecting! Not the most thorough review (migraine winning)

---

_Review comment by @MichaReiser on `crates/ty_server/src/session.rs`:1571 on 2025-12-10 16:40_

Can't disagree. All our 'Keep the world in sync with salsa' is a bit brittle. Not sure what else to do. 

---

_@MichaReiser reviewed on 2025-12-10 16:40_

---

_Merged by @MichaReiser on 2025-12-10 16:47_

---

_Closed by @MichaReiser on 2025-12-10 16:47_

---

_Branch deleted on 2025-12-10 16:47_

---
