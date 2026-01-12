```yaml
number: 19533
title: upgrade to schemars 1.0.0
type: pull_request
state: closed
author: CodeMan62
labels: []
assignees: []
draft: true
base: main
head: codeman/19173
created_at: 2025-07-24T17:55:23Z
updated_at: 2025-09-29T13:56:28Z
url: https://github.com/astral-sh/ruff/pull/19533
synced_at: 2026-01-12T15:56:41Z
```

# upgrade to schemars 1.0.0

---

_@CodeMan62_

## Summary
closes #19173 
in schemars 1.0.0 there are some breaking changes we need to adopt them 
## Test Plan
from contributing.md

---

_Comment by @github-actions[bot] on 2025-07-27 11:00_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @CodeMan62 on 2025-07-28 05:58_

---

_Review requested from @carljm by @CodeMan62 on 2025-07-28 05:58_

---

_Review requested from @AlexWaygood by @CodeMan62 on 2025-07-28 05:58_

---

_Review requested from @sharkdp by @CodeMan62 on 2025-07-28 05:58_

---

_Review requested from @dcreager by @CodeMan62 on 2025-07-28 05:58_

---

_Review requested from @MichaReiser by @CodeMan62 on 2025-07-28 05:58_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-28 06:51_

---

_Comment by @MichaReiser on 2025-07-28 07:50_

Thanks for working on this. I think there's a little more to do and I'll put this back into draft because of it. It seems there are many semantical schema changes when there should be none

---

_Converted to draft by @MichaReiser on 2025-07-28 07:50_

---

_Comment by @github-actions[bot] on 2025-08-04 05:43_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_@CodeMan62 reviewed on 2025-08-09 18:47_

---

_Review comment by @CodeMan62 on `crates/ty_project/src/files.rs`:64 on 2025-08-09 18:47_

it was throwing a irrelevant error is this okay to do this ??
EDIT:- Mistake

---

_Marked ready for review by @CodeMan62 on 2025-08-10 16:21_

---

_Comment by @CodeMan62 on 2025-08-10 16:52_

Now there is no semantic schema changes everything is done almost all tests passing some are failing due to some warning which i am unable to solve as you can see that i have tried so a little help can make all the work done here 

---

_Comment by @github-actions[bot] on 2025-08-10 16:56_

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

_Review comment by @MichaReiser on `ty.schema.json`:282 on 2025-08-11 08:21_

It seems that `RangedValue` now gets repeated multiple times

---

_Review comment by @MichaReiser on `ty.schema.json`:158 on 2025-08-11 08:24_

I think adding the newlines is a breaking change in schemars. We should test if the hover rendering in IDEs is unchanged

---

_Review comment by @MichaReiser on `ty.schema.json`:160 on 2025-08-11 08:25_

Is the switch from `enum` to `full` due to a change in schemars or is it due to your changes? If so, what's the motivation for it

---

_Review comment by @MichaReiser on `ruff.schema.json`:12 on 2025-08-11 08:26_

This is low priority but given (as far as I remember) that this is manually generated. Any chance we could preserve the property order so that `deprecated comes between `description` and `type` (to reduce the diff size)

---

_Review comment by @MichaReiser on `ruff.schema.json`:1976 on 2025-08-11 08:28_

Can we double check if it's correct that the description is missing here?

---

_Review comment by @MichaReiser on `ruff.schema.json`:4295 on 2025-08-11 08:29_

This seems unnecessary

---

_@MichaReiser reviewed on 2025-08-11 08:29_

Thank you. There are still a few semantic differences that we should look into and test how the new schema behave in editors 

---

_@CodeMan62 reviewed on 2025-08-11 14:44_

---

_Review comment by @CodeMan62 on `ty.schema.json`:282 on 2025-08-11 14:44_

yes this is because schemars now creating seprate schema definitions for each generic instantiation of `RangedValue<T> ` instead of trying to reuse a single generic definition

---

_@CodeMan62 reviewed on 2025-08-11 14:45_

---

_Review comment by @CodeMan62 on `ty.schema.json`:158 on 2025-08-11 14:45_

can you please do this for me or tell me how i can do this ? 

---

_@CodeMan62 reviewed on 2025-08-11 14:48_

---

_Review comment by @CodeMan62 on `ty.schema.json`:160 on 2025-08-11 14:48_

this is due to schemars. 

---

_Converted to draft by @CodeMan62 on 2025-08-13 16:21_

---

_Comment by @CodeMan62 on 2025-08-13 16:21_

converting to draft as i am trying to achieve the last third reviews 

---

_@CodeMan62 reviewed on 2025-08-21 17:46_

---

_Review comment by @CodeMan62 on `ruff.schema.json`:12 on 2025-08-21 17:46_

I am continously trying to do but i think i am missing something can we create a followup? 

---

_@CodeMan62 reviewed on 2025-08-21 18:02_

---

_Review comment by @CodeMan62 on `ruff.schema.json`:1976 on 2025-08-21 18:02_

Before the changes, `LintOptions` and `Options` properties had their separate description, but now both have one description in `Options` because we have to derive `Jsonschema` at `LintOptionsWire`. I can't figure out what to do here any guesses? 

---

_Marked ready for review by @CodeMan62 on 2025-08-21 18:02_

---

_@MichaReiser requested changes on 2025-08-22 06:11_

Thank you. Can you take a look at why all the CI jobs are failing

---

_Comment by @CodeMan62 on 2025-08-22 06:13_

Sure I will fix it asap 

---

_Converted to draft by @MichaReiser on 2025-08-22 06:52_

---

_Comment by @CodeMan62 on 2025-08-22 18:24_

now most of the CI that was failing is fixed now according to the current CI fails there is issue of schema generation 
>     test generate_ty_schema::tests::test_generate_json_schema ... FAILED

which is not possible you can have a look at diff too and the tests are passing locally too so the PR is complete from my side 
NOTE:- Schemas are updated and pushed already 

---

_Marked ready for review by @CodeMan62 on 2025-08-22 18:24_

---

_Converted to draft by @CodeMan62 on 2025-09-09 15:14_

---

_Comment by @CodeMan62 on 2025-09-29 13:56_

I am really sorry for not making it forward i have tried my best. again sorry for this closing it 

---

_Closed by @CodeMan62 on 2025-09-29 13:56_

---
