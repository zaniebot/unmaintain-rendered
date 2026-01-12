```yaml
number: 21044
title: "[ty] Improve `invalid-argument-type` diagnostics where a union type was provided"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/union-diag
created_at: 2025-10-23T10:56:30Z
updated_at: 2025-10-27T08:26:07Z
url: https://github.com/astral-sh/ruff/pull/21044
synced_at: 2026-01-12T15:57:15Z
```

# [ty] Improve `invalid-argument-type` diagnostics where a union type was provided

---

_@AlexWaygood_

Fixes https://github.com/astral-sh/ty/issues/1411

---

_Review requested from @carljm by @AlexWaygood on 2025-10-23 10:56_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-10-23 10:56_

---

_Review requested from @dcreager by @AlexWaygood on 2025-10-23 10:56_

---

_Label `ty` added by @AlexWaygood on 2025-10-23 10:56_

---

_Label `diagnostics` added by @AlexWaygood on 2025-10-23 10:56_

---

_Label `ty` added by @AlexWaygood on 2025-10-23 10:56_

---

_Label `diagnostics` added by @AlexWaygood on 2025-10-23 10:56_

---

_Comment by @github-actions[bot] on 2025-10-23 10:58_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-23 10:59_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/snapshots/function.md_-_Call_expression_-_Wrong_argument_type_-_Diagnostics_for_unio…_(5396a8f9e7f88f71).snap`:47 on 2025-10-23 11:50_

The secondary info message won't be shown with `--output-format=concise` or in the main diagnostic shown in editors (unless they supported related information which many dont). 

Given that this is the most important bit, I'm inclined to make it part of the primary annotation message, although that's somewhat tricky

---

_@MichaReiser reviewed on 2025-10-23 11:50_

---

_@AlexWaygood reviewed on 2025-10-23 11:58_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/function.md_-_Call_expression_-_Wrong_argument_type_-_Diagnostics_for_unio…_(5396a8f9e7f88f71).snap`:47 on 2025-10-23 11:58_

I think we may have had this conversation before... I think we may need to rethink our diagnostic model here. It's often not feasible to encapsulate all relevant information in a single line, and mypy, for example, will often display secondary diagnostics even if you do not specify `--pretty`. (Mypy uses a concise output format by default; `--pretty` gives you annotations as part of diagnostics, similar to our output format.) E.g. https://mypy-play.net/?mypy=latest&python=3.12&gist=8322703266ffc5e8e2ccb9538e2487c3

The approach I'm taking here is also exactly what we already do in similar diagnostics, so I'm inclined to say that if we want to change this approach, we should do so for all our diagnostics together: https://github.com/astral-sh/ruff/blob/01695513ce33f1f1615309323ba145c42f4720c1/crates/ty_python_semantic/resources/mdtest/snapshots/union_call.md_-_Calling_a_union_of_f%E2%80%A6_-_A_smaller_scale_exam%E2%80%A6_(c24ecd8582e5eb2f).snap#L34-L53

---

_@MichaReiser reviewed on 2025-10-23 12:02_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/snapshots/function.md_-_Call_expression_-_Wrong_argument_type_-_Diagnostics_for_unio…_(5396a8f9e7f88f71).snap`:47 on 2025-10-23 12:02_

It's probably worth changing it everywhere because using sub diagnostics with info won't show with `--output-format=concise` or in the editor. The reason I opened this issue was because I didn't had enough information in the editor to fix the diagnostic. 

I'd be inclined to change the message to:

```
Expected `Sized`, element `Foo` of `str | Foo` is not assignable to `Sized`


---

_@AlexWaygood reviewed on 2025-10-23 12:06_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/function.md_-_Call_expression_-_Wrong_argument_type_-_Diagnostics_for_unio…_(5396a8f9e7f88f71).snap`:47 on 2025-10-23 12:06_

> using sub diagnostics with info won't show with `--output-format=concise`

yes, that's the thing I'm suggesting we might want to rethink when I said "we may need to rethink our diagnostic model here". I don't think that's a great outcome. I think it's going to end up with worse diagnostics than mypy for CLI users. It's often silly to try to cram all relevant information onto one line, and for things like overloads it's arguably impossible.

> or in the editor

Whenever I need more information on a diagnostic rust-analyzer is showing me in the "Problems" tab in VSCode, I always click on the "see full diagnostic" option, and then a new tab opens up with all subdiagnostics displayed as well as the main diagnostic. Is it not possible for us to implement something similar for users of the ty server?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/function.md_-_Call_expression_-_Wrong_argument_type_-_Diagnostics_for_unio…_(5396a8f9e7f88f71).snap`:47 on 2025-10-23 12:11_

And it seems to me that I'm already being shown several subdiagnostics on-hover here in VSCode, for example, even without clicking on the "Click for full compiler diagnostic" option?

<img width="2332" height="850" alt="image" src="https://github.com/user-attachments/assets/d51a4540-53a3-4831-b9b9-2bafb47ce79f" />


---

_@AlexWaygood reviewed on 2025-10-23 12:11_

---

_@MichaReiser reviewed on 2025-10-23 12:14_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/snapshots/function.md_-_Call_expression_-_Wrong_argument_type_-_Diagnostics_for_unio…_(5396a8f9e7f88f71).snap`:47 on 2025-10-23 12:14_

> Whenever I need more information on a diagnostic rust-analyzer is showing me in the "Problems" tab in VSCode, I always click on the "see full diagnostic" option, and then a new tab opens up with all subdiagnostics displayed as well as the main diagnostic. Is it not possible for us to implement something similar for users of the ty server?

We do implement this but it's not supported by all editors

> I think it's going to end up with worse diagnostics than mypy for CLI users. 

Only when using `--output-format=full`. 

I do share the sentiment that we shouldn't try to put everything on a single line. But I think it's crucial that the most important bits are visible on that single line. In this case, it's not the union, but which element of the union doesn't implement `Sized`. We can move the union out of the main message into a secondary info message. But I think we need to have the problematic element in the primary message.



---

_@AlexWaygood reviewed on 2025-10-23 12:20_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/function.md_-_Call_expression_-_Wrong_argument_type_-_Diagnostics_for_unio…_(5396a8f9e7f88f71).snap`:47 on 2025-10-23 12:20_

> `` Expected `Sized`, element `Foo` of `str | Foo` is not assignable to `Sized` ``

I think if I'm a user, this message could easily raise more questions than it answers. The user might not have any idea why we're talking about either `Foo` or `Foo | str` -- we haven't yet told them that we inferred a type of `str | Foo` for the argument they passed into the function call. Users think in terms of values, not types; all they'll see in their source code is a variable `bar`, and they might not even be aware of what type `bar` has at runtime, let alone the union type we've inferred for it in ty. I don't think we can start talking about which specific element in the union is causing an issue before we've told them that we've inferred a union type for the argument being passed in

---

_@AlexWaygood reviewed on 2025-10-23 12:21_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/function.md_-_Call_expression_-_Wrong_argument_type_-_Diagnostics_for_unio…_(5396a8f9e7f88f71).snap`:47 on 2025-10-23 12:21_

> We do implement this but it's not supported by all editors

Feels like a feature request that should be brought up with those editors, since it'll also lead to a bad experience when using other LSP servers like rust-analyzer ;)

---

_@AlexWaygood reviewed on 2025-10-23 12:23_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/function.md_-_Call_expression_-_Wrong_argument_type_-_Diagnostics_for_unio…_(5396a8f9e7f88f71).snap`:47 on 2025-10-23 12:23_

This discussion does make me wonder whether it might be worth revisiting my original idea in https://github.com/astral-sh/ruff/pull/20730#discussion_r2410446248 of having a "smart filter" option for union displays (so that we can make sure at least one "problematic union element" is always included in the display)

---

_@MichaReiser reviewed on 2025-10-23 12:25_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/snapshots/function.md_-_Call_expression_-_Wrong_argument_type_-_Diagnostics_for_unio…_(5396a8f9e7f88f71).snap`:47 on 2025-10-23 12:25_

> Feels like a feature request that should be brought up with those editors, since it'll also lead to a bad experience when using other LSP servers like rust-analyzer ;)

Maybe, but that's out of our control and it's still an issue when using `--output-format=concise`. 

> This discussion does make me wonder whether it might be worth revisiting my original idea in https://github.com/astral-sh/ruff/pull/20730#discussion_r2410446248 of having a "smart filter" option for union displays (so that we can make sure at least one "problematic union element" is always included in the display)

I'd sort of like this more. It seems very unfortunate to just truncate anywhere. I think it also degraded the LSP experience because it's now impossible to see the full type when hovering (and I assume we also truncate when using `reveal_type`?)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/function.md_-_Call_expression_-_Wrong_argument_type_-_Diagnostics_for_unio…_(5396a8f9e7f88f71).snap`:47 on 2025-10-23 12:26_

> and I assume we also truncate when using `reveal_type`?

no, we do not. There's an option to switch off truncation when displaying types and `reveal_type` already uses it. The type displayed for on-hover should probably also use it; this was probably an oversight in #20730!

---

_@AlexWaygood reviewed on 2025-10-23 12:26_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/snapshots/function.md_-_Call_expression_-_Wrong_argument_type_-_Diagnostics_for_unio…_(5396a8f9e7f88f71).snap`:47 on 2025-10-23 12:27_

It's probably worth exploring how other type checkers (across ecosystem) handle long types (and union types) in diagnostics and editors

---

_@MichaReiser reviewed on 2025-10-23 12:27_

---

_@AlexWaygood reviewed on 2025-10-23 12:27_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/function.md_-_Call_expression_-_Wrong_argument_type_-_Diagnostics_for_unio…_(5396a8f9e7f88f71).snap`:47 on 2025-10-23 12:27_

Though even for on-hover, we may want to avoid displaying all union elements in situations where the union is [60 elements long](https://github.com/astral-sh/ruff/pull/20730#issuecomment-3381454378)...?

---

_@AlexWaygood reviewed on 2025-10-23 12:34_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/function.md_-_Call_expression_-_Wrong_argument_type_-_Diagnostics_for_unio…_(5396a8f9e7f88f71).snap`:47 on 2025-10-23 12:34_

In general I think we infer unions more than any other Python type checker. This is not only because we treat intersections as first-class types, but also because we have concepts such as "class-literal types" and "function-literal types". To my knowledge, no other Python type checker treats these concepts as first-class types, and treating them as such seems to mean that huge unions are more common for us than for other type checkers

---

_@MichaReiser reviewed on 2025-10-23 12:34_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/snapshots/function.md_-_Call_expression_-_Wrong_argument_type_-_Diagnostics_for_unio…_(5396a8f9e7f88f71).snap`:47 on 2025-10-23 12:34_

<img width="1895" height="440" alt="Screenshot 2025-10-23 at 14 32 24" src="https://github.com/user-attachments/assets/a22de7d9-13c4-44d0-bece-149709694e8c" />

Typescript does truncate very long unions

<img width="1248" height="1325" alt="Screenshot 2025-10-23 at 14 34 04" src="https://github.com/user-attachments/assets/212c0c00-3f8d-4ed8-b7fa-72a4023b1a1e" />

https://www.typescriptlang.org/play/?#code/PTAEDEFcDsGMBcCWB7ap4AsCG9SoDYCeoWssApgA7wDOoN8ATotAOY0BQAZjAimpWbR4AZSYtWACgBuWfJHIAueuLYBKZdOSIAJqADeHUKFioayfOQB0+ZFNnzyagNwcAvhw4hQAFQyI6AHdEfHxQckZGZEZQACNyWCxIGnJ0DFSsaFRCAFtkZNAYfhNUeCwWOgAWACZuXiRUdHIGSSNQRHhyHOU24wAfUAAiLEpKS0He0AHB2My5ieN+odh0yMIFxemdHHINpcHyfB0I+LW9qaGuRFZz6dZGEd3J6YxUckJjwNuhgGtEYO+g0seWggJymVYyEB0ASZSET0WF0G0QhCM2Q0oIywhCwgIAjpAWBRAQ8aJRTox1s8hgwHoEKVTEdMymwIiw0ftAjtGDlDqhASNmKZ4IDYvhSD8GaLHFLqYNEsI5PlKBykaZTNBICK5bAHtBZUyhjoHpDoFxGITtYbBpDkCkDeibQ8VebLYDWJAsLJAQArCWujqA-CIXlBwgrciq6bg6DbIRgyD4B37CyIaRRjHkUgYQEqrCMXMRGghkG59kjMYZwZjSA5XPIXn3OadXMWmEk8rF0Fyhj5gNWx3wBs4KHUmrGbwAOVrp3aaEwqSKqAAhBwNKAtLoDG1BCxRKopB0ui5QN4AKKRaLKTU5WcBUBZXBYGjF1jQLBi1JDlRCVirjxeGAZ7pmgeSMKkpg5JWAAeyiZNkeQFEuaDBJgeCxD6sLoIQKqcDwcANAIUQUC+ADCDYwQAkp0OStMYR7dNS+jtDo14zhEzgPlgvLKLSEicaQSDpsosTIBYWZoB4hrMbobG3hx6AdJYvEHpxlCQGKATpKxcRiZYmSgFJ6IyTpN6nJx4rxPgKm-px0gBIgn4iXpEmGUxLFyeZoDHDQuqINQ-A2fx4Tvp+OmieJBlGUsJmeQpZSsEFbCcVwWbwJA4HhS5UXubJD7sYwnGJJ0kKUklrACYwKxpuQWWRZJuWmQVdlyAo5WcSklgILVzn1W50kefl8mFSUwjkMI7UkKMUTpnV+kNQNeVmfF5DQfAk3pswVw9bpfXRRcsVDV5iQBagk3+KwGDBpdnRza5+0DIdy0jekWA6BIk27uCZW7fN-XGYNz0dRpr3vWwk0pBqcaEL1f0PQYgPNfQtbfTDP7BatmKxjtEVw41cUjY+Sjo8loCUCwMJ3Tli1NcNRUNryE0kxVoBcOKrCsDj2ULQDS1I7yL5YJzk3gW9sP3fjR0KTQGmYQgENwpl4vU7ztNeUO5OwJNTDjWDiW-RLNME5xPzvIE0Q6XxpNMBKXN7ZLQNkxgpLE1bLMyxzzS3crPMxYjdOgASyCdJNXBetER5U77B3+15NDQP5KrrcznERT832m1H-1+3zAerRQjDUBD2BKwbKs52rCmYiazoYJNxpYFw3tl9Hj2x9L42dHAruqSUUGWM3uOG6rxugObjCW73gjICRKRZ-DT1I50PKTSwxzQXbeNG1LI3HFcCeESL5D2eQgSb8PFej5j4rvofKcbnIug7PPDtI7A4pbYgxWBffGpXDy59y4x1zl5CoTBIB8DOvfVaCQtSANbgjEBCl3rgT4MJe+IZKyM0HtzbOwDK4jQ9LoQ47JQ4WFsGfF+29HYWmUtAs00QKBUJHjvNSqZYBozdgJUYwZ4F4Lbkgka08mEZR7rZbyM9aydz4QvduQiohDlMNZaBvZNI0G0j7fhiCCEdRZHGSe4i3rIGoDI1+AcyQJEQFcb+UCuHeXIPvUx1CkbgQJIgcC2CIY4ACNtZhl9WEmGYMvH+djeTJyHkAgROjRrvTvnYng+ArihCcSwx2GpaTlCZnY8CFjup+PwaPYMOQOjeNsb3CUWRAiWB0JzfJUTR6mGOKPBwT8cF7QnGAAA5I08gnT2h0Gequdcm49CGA6b4Sk6BkAkDIM0OgWAyZRBVIweAxBMA4HCNBAItA4halANgOgRIKLeM-NhXCbR0m4B6QANVamIiQoAAC87QaJWB6ZxU8QFLyMH-EAA


---

_@AlexWaygood reviewed on 2025-10-23 12:47_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/function.md_-_Call_expression_-_Wrong_argument_type_-_Diagnostics_for_unio…_(5396a8f9e7f88f71).snap`:47 on 2025-10-23 12:47_

No other Python type checker attempts to highlight the problematic union element as part of the _primary_ diagnostic for something like this, from what I can tell:
- Mypy: https://mypy-play.net/?mypy=latest&python=3.12&gist=e52205a0769db5dd033a163d85f561f8
- Pyright: https://pyright-play.net/?pyrightVersion=1.1.405&strict=true&code=CYUwZgBGAUAeBcECWA7ALhAPhAzmgTgJTwCwAUBJRADYgpyHlA
- Pyrefly: https://pyrefly.org/sandbox/?project=N4IgZglgNgpgziAXKOBDAdgEwEYHsAeAdAA4CeSImMYABGABT6I0ToAuNAPjXGwE4BKRAB10NcTVjpGA0SAA0IAK5tocEuUQgAxDQCqqqBDak6S9AGNVudHFGiqtMLj4BbVGwD66Ja%2Bww%2BRmZWNgEaAFoAPh5%2BETEJPhg2JT4xMGEQADlffz5mYHwAXwy5RTJEsChSQjZcVygKXQAFUgqqngwcAhoLG0gAcxSPCBtCUV0AZRgYGgALNjZiOEQAehXy6irCF36VmHQVzFwLOBXe9AGh6wO6FxpUADdUaFRsWB6%2BiEG%2BYZsaXGI13UojIbFmNnCDwCcBGYgAvDQMgBmQgARgATCV0CBCopUFYIFCAGLQGAUNBYPBEMg4oA

Pyright highlights it in a subdiagnostic, the same as my PR currently proposes

---

_@MichaReiser reviewed on 2025-10-23 13:01_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/snapshots/function.md_-_Call_expression_-_Wrong_argument_type_-_Diagnostics_for_unio…_(5396a8f9e7f88f71).snap`:47 on 2025-10-23 13:01_

That's true. But no other type checker seems to truncate the union type :)

I'm fine merging this as is but I really think our approach to truncating unions is problematic

---

_@MichaReiser approved on 2025-10-23 13:01_

---

_Merged by @AlexWaygood on 2025-10-23 13:16_

---

_Closed by @AlexWaygood on 2025-10-23 13:16_

---

_Branch deleted on 2025-10-23 13:16_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/call/bind.rs`:3550 on 2025-10-27 08:19_

I assumed we would fix this somewhere else, e.g. when actually checking assignability in the first place. This code here only affects invalid-argument-type, but we should do the same thing in other situations, like for actual assignments to variables. Consider:
```py
from typing import Sized

class Foo: ...

def g(
    b: list[str]
    | str
    | dict[str, str]
    | tuple[str, ...]
    | bytes
    | frozenset[str]
    | set[str]
    | Foo,
):
    y: Sized = b  # still only shows the truncated union
```



---

_@sharkdp reviewed on 2025-10-27 08:19_

---

_@AlexWaygood reviewed on 2025-10-27 08:26_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:3550 on 2025-10-27 08:26_

A more general solution would probably be https://github.com/astral-sh/ruff/pull/19580 (so that we always propagate up the reason _why_ an assignability/subtyping relation did not hold). I do think we need something like that eventually, for better diagnostics in cases where Protocol subtyping doesn't hold (our diagnostics are currently terrible for protocols with lots of members — you can't tell which specific member of the protocol wasn't satisfied). But that would be a big change, and I'm not yet sure how it would fit in with Doug's work on the new constraint solver.

We already do ad-hoc special-casing of union types like this for several other diagnostics. I agree it's not great but I think it's the best we can do for now, and it's better than nothing in the short term.

---
