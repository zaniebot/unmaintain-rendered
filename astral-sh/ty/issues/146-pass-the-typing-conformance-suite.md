---
number: 146
title: Pass the typing conformance suite
type: issue
state: open
author: carljm
labels:
  - typing semantics
assignees: []
created_at: 2025-03-27T18:41:57Z
updated_at: 2026-01-09T15:28:04Z
url: https://github.com/astral-sh/ty/issues/146
synced_at: 2026-01-10T01:48:23Z
---

# Pass the typing conformance suite

---

_Issue opened by @carljm on 2025-03-27 18:41_

...or explicitly document the cases where we intentionally choose to diverge from it? (In which case we should advocate for a change to / relaxation of the spec.)

---

_Renamed from "[red-knot] pass the typing conformance suite" to "pass the typing conformance suite" by @MichaReiser on 2025-05-07 15:25_

---

_Label `typing semantics` added by @AlexWaygood on 2025-05-11 07:57_

---

_Renamed from "pass the typing conformance suite" to "Pass the typing conformance suite" by @dhruvmanila on 2025-07-18 14:14_

---

_Comment by @AlexanderWillner on 2025-10-14 18:58_

Based on [How Well Do New Python Type Checkers Conform?](https://sinon.github.io/future-python-type-checkers/) we see [that ty 0.0.1-alpha.19 passes 20 tests](https://htmlpreview.github.io/?https://github.com/sinon/typing/blob/058ea116b6d8d98d3372c775865cec9f4f9e45c3/conformance/results/results.html) 

---

_Comment by @oconnor663 on 2025-12-05 00:29_

Carl has pointed out some overly-strict asserts in the conformance suite that we might want to submit a PR upstream for. Copying from chat:

> the relevant tests (which we still fail, due to the over-specification of assert_type) are at https://github.com/python/typing/blob/main/conformance/tests/generics_defaults.py#L30 and following
>
> for us, an unspecialized mention of the value NoNonDefaults is just a singleton "class object" type, which is a subtype of type[NoNonDefaults[str, int]], but not equivalent to it
>
> but these conformance tests (IMO) over-specify here
>
> they should instead just test that if you type-annotate something as NoNonDefaults, you get a type of NoNonDefaults[str, int] for that name (which is true for us)
>
> https://play.ty.dev/e871464d-4c6e-4d71-b032-b055f0192997
>
> Obviously that still uses assert_type -- I think the real problem isn't using assert_type, it's that "the type of a generic class object as a value" is not well-specified in the Python type system, and the conformance suite shouldn't implicitly assert that it must be a default-specialized type[] type

---

_Comment by @carljm on 2025-12-05 18:54_

In https://github.com/astral-sh/ty/issues/1762#issuecomment-3616910856 @AlexWaygood suggests a workaround we could implement in our `assert_type` handling that could be an alternative to changing these `assert_type` usages upstream. (The rationale is that this issue doesn't only impact the conformance suite, it also impacts other people using `assert_type` to test their stubs.)

---

_Comment by @tsudol-plaid on 2025-12-17 19:57_

Now that ty is in beta (congrats!!!!) are there plans to add it to the conformance test suite?

---

_Comment by @zanieb on 2025-12-17 20:01_

Yep, we're planning on working on it.

---

_Removed from milestone `Stable` by @carljm on 2025-12-19 23:38_

---

_Added to milestone `M1` by @carljm on 2025-12-19 23:38_

---

_Assigned to @AlexWaygood by @carljm on 2026-01-09 15:28_

---
