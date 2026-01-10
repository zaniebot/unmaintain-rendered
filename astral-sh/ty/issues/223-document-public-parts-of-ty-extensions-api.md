---
number: 223
title: Document public parts of ty_extensions API
type: issue
state: open
author: sharkdp
labels:
  - documentation
  - needs-design
assignees: []
created_at: 2025-01-08T14:09:25Z
updated_at: 2026-01-09T22:41:50Z
url: https://github.com/astral-sh/ty/issues/223
synced_at: 2026-01-10T01:48:23Z
---

# Document public parts of ty_extensions API

---

_Issue opened by @sharkdp on 2025-01-08 14:09_

Quoting some comments that were spread over multiple threads in [this PR](https://github.com/astral-sh/ruff/pull/15103):

@MichaReiser 

> What are your thoughts on whether we should gate this behind a feature or make it testing only?

@sharkdp 

> Seeing that mypy and pyre have similar public APIs, I could imagine (part of) this becoming a public API for red knot that isn't feature-gated? The `Intersection`/`Not` constructors and `static_assert` are potentially interesting for users which don't care too much about compatibility with other type checkers (cf https://github.com/python/typing/issues/213). Some less-interesting parts like `is_singleton` could also be moved to `knot_extensions.internal` maybe?

> Do we need to decide this right away? If not, can this stay public for now, or should we approach it very carefully and hide everything (by default)?

@MichaReiser 

> I'm generally leaning toward keeping the API intentionally limited and only expose features that we want to support long term (and consider part of the public API) because users will using them and it's very easy that we forget to revisit this decision before the release (unless we note it down somewhere?)

@carljm 

> For what it's worth, there's nothing in the API added in this PR that I have reservations about supporting long-term as public API, in principle. Of course it's always possible that we realize we made mistakes in details of how we defined or named the API, and we want to change these things in future and potentially have to take backward-compatibility into account with those changes. But this is inevitable.

And another comment by @carljm regarding how this could be implemented technically:

> I think whatever feature gating we do should probably not happen at the Rust implementation level (that is, trying to turn off or on certain Rust code paths in type inference), but rather at the Python level; that is, in the type stub for the knot_extensions module. This could mean only conditionally including the type stub for knot_extensions at all, or having some conditional definitions of things in the type stub. If you can't import the known-function and known-instance types that make up the type API, then it doesn't matter whether the code paths exist for those; you won't be able to enter those code paths anyway.

---

_Comment by @AlexWaygood on 2025-01-14 18:28_

I wonder if one way of doing this would just be to issue a diagnostic whenever we see an import from `knot_extensions` in code that we're checking (which warns you that it's (a) experimental and (b) doesn't exist at runtime), but have that diagnostic switched off by default in the context of our test suite?

---

_Comment by @sharkdp on 2025-01-14 21:09_

I like that! I guess the only downside I can think of right now is that we might want to deprecate that lint rule eventually *if* we ever want to "stabilize" `knot_extensions`.

---

_Comment by @MichaReiser on 2025-01-14 21:35_

Would this be a regular lint that you can disable? If so, what category would it belong to long term?

I think it should not be a regular rule. We don't want it to show up in configurations etc. While I designed for this possibility, not everything is there yet to actually support this distinction. 

---

_Comment by @carljm on 2025-01-14 21:36_

I'm not sure we need to do anything at all about this issue; I don't think it's a priority to spend time on it right now. 

---

_Renamed from "[red-knot] Hide parts of the `knot_extensions` API?" to "Hide parts of the `knot_extensions` API?" by @MichaReiser on 2025-05-07 15:27_

---

_Label `needs-design` added by @AlexWaygood on 2025-05-11 10:51_

---

_Renamed from "Hide parts of the `knot_extensions` API?" to "Hide parts of the `ty_extensions` API?" by @MichaReiser on 2025-08-15 12:23_

---

_Added to milestone `Stable` by @carljm on 2025-11-13 06:24_

---

_Comment by @MichaReiser on 2026-01-09 09:36_

I wonder if we should close this. To me, they very much feel like public API at this point, given that we sometimes even suggest them in issue responses (e.g. `Intersection)` 

---

_Comment by @AlexWaygood on 2026-01-09 10:26_

Several of them really are just intended for internal mdtest use, though; we _should_ probably move them to a private submodule 

---

_Comment by @carljm on 2026-01-09 20:09_

I'm curious specifically which ones you'd move. Even the stuff we mostly use in mdtests (`assert_subtype_of` etc) I can see interested users finding a use for. Even so I guess it wouldn't hurt to mark them as private -- motivated users could use them anyway.

I guess the highest-value ones to move are ones we might change significantly. E.g. the constraint-set ones changed several times already, I think.

---

_Comment by @AlexWaygood on 2026-01-09 20:16_

yeah... I think it's just a big backwards-compatibility burden to say that they're all totally frozen now in terms of their APIs? Things like `Intersection` and `Not` are obviously useful and are obviously not going to have their APIs changed, but I don't really want to tie our hands for something like `ty_extensions.reveal_protocol_interface`. I'd like to be able to continue to freely change the output/expected parameters of that in the future -- it would just be nice to make it clear that there are no backwards-compatibility guarantees for the more internal things like that.

---

_Comment by @MichaReiser on 2026-01-09 21:30_

To me that sounds more like we should establish which API we consider to be public. For now, I wouldn't consider any API in ty_extensions to fall under our versioning policy. 

So I think what this issue now really is about is documenting which parts are official and versioned rather than necessary hiding any of it.

---

_Renamed from "Hide parts of the `ty_extensions` API?" to "Document public parts of ty_extensions API" by @carljm on 2026-01-09 22:41_

---

_Label `documentation` added by @carljm on 2026-01-09 22:41_

---
