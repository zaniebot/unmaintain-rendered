```yaml
number: 20850
title: "[ty] Do not bind self to non-positional parameters"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/fix-1333
created_at: 2025-10-13T18:25:02Z
updated_at: 2025-10-13T18:53:40Z
url: https://github.com/astral-sh/ruff/pull/20850
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Do not bind self to non-positional parameters

---

_Pull request opened by @sharkdp on 2025-10-13 18:25_

## Summary

closes https://github.com/astral-sh/ty/issues/1333

## Test Plan

Regression test

---

_Label `ty` added by @sharkdp on 2025-10-13 18:25_

---

_Marked ready for review by @sharkdp on 2025-10-13 18:25_

---

_Review requested from @carljm by @sharkdp on 2025-10-13 18:25_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-13 18:25_

---

_Review requested from @dcreager by @sharkdp on 2025-10-13 18:25_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-13 18:25_

---

_Comment by @github-actions[bot] on 2025-10-13 18:27_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_@AlexWaygood approved on 2025-10-13 18:27_

---

_Comment by @github-actions[bot] on 2025-10-13 18:28_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @AlexWaygood on 2025-10-13 18:30_

> ## `mypy_primer` results
> No ecosystem changes detected ✅ No memory usage changes detected ✅

Well, it still makes sense as a standalone change!

---

_Comment by @github-actions[bot] on 2025-10-13 18:30_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results

No diagnostic changes detected ✅
**[Full report with detailed diff](https://david-fix-1333.ecosystem-663.pages.dev/diff)** ([timing results](https://david-fix-1333.ecosystem-663.pages.dev/timing))


---

_Merged by @sharkdp on 2025-10-13 18:44_

---

_Closed by @sharkdp on 2025-10-13 18:44_

---

_Branch deleted on 2025-10-13 18:44_

---

_Comment by @sharkdp on 2025-10-13 18:46_

> > No ecosystem changes detected ✅ No memory usage changes detected ✅
> 
> Well, it still makes sense as a standalone change!

I didn't quite understand why you thought most of the changes came from the fix, but I agree that it can be a standalone PR ;-)

(the fix *was* necessary in order to remove 1139 diagnostics that would otherwise be caused in combination with https://github.com/astral-sh/ruff/pull/20802)

---

_Comment by @AlexWaygood on 2025-10-13 18:49_

> I didn't quite understand why you thought most of the changes came from the fix

Yes, I obviously misunderstood. Sorry!

---

_Comment by @sharkdp on 2025-10-13 18:53_

> > I didn't quite understand why you thought most of the changes came from the fix
> 
> Yes, I obviously misunderstood. Sorry!

No worries, good suggestion nevertheless

---
