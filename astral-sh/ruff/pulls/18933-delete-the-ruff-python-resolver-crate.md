```yaml
number: 18933
title: "Delete the `ruff_python_resolver` crate"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/delete-ruff-resolver
created_at: 2025-06-25T06:28:33Z
updated_at: 2025-06-25T10:53:15Z
url: https://github.com/astral-sh/ruff/pull/18933
synced_at: 2026-01-10T18:39:09Z
```

# Delete the `ruff_python_resolver` crate

---

_Pull request opened by @MichaReiser on 2025-06-25 06:28_

## Summary

We now have a working module resolver in `ty_python_semantic`. `ruff_python_resolver` is unused, has very little test coverage, and gives the wrong impression that this could be used in ruff.

We didn't delete this in the past because we wanted it as a reference for ty's module resolver. I think we're now at a point where this is no longer necessary (ty's module resolver is further along). We can also always take a look at pyright's module resolver, from which this implementation was inspired from.

## Test Plan

`cargo build`


---

_Label `internal` added by @MichaReiser on 2025-06-25 06:28_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-06-25 06:29_

---

_Comment by @github-actions[bot] on 2025-06-25 06:33_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@sharkdp approved on 2025-06-25 06:34_

---

_Comment by @github-actions[bot] on 2025-06-25 06:45_

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

_Renamed from "Delete the `ruff_python_resovler` crate" to "Delete the `ruff_python_resolver` crate" by @AlexWaygood on 2025-06-25 09:27_

---

_Comment by @AlexWaygood on 2025-06-25 09:30_

I still feel like this will be a useful reference for some of the more advanced features pyright's resolver has that ty's doesn't yet have, but which we want to add in the future. For example, pyright's resolver has the ability to resolve a module to a C extension, and therefore records the fact that the module does actually exist at runtime, pyright just has no types for it. That allows pyright to issue different diagnostics for "the module isn't installed" vs "the module is installed but you'll need to install some stubs before I can do type inference properly".

I guess this crate will always be in Ruff's git history if you _really_ want to remove it _now_, though 😛

---

_Comment by @MichaReiser on 2025-06-25 10:33_

> For example, pyright's resolver has the ability to resolve a module to a C extension, and therefore records the fact that the module does actually exist at runtime, pyright just has no types for it. That allows pyright to issue different diagnostics for "the module isn't installed" vs "the module is installed but you'll need to install some stubs before I can do type inference properly"

If that's the only use, then I suggest that we look at Pyright's module resolver implementation (which was the inspiration for our implementation). https://github.com/microsoft/pyright/blob/e5ecd9e32f54db97222a982db26fb182c34125d8/packages/pyright-internal/src/analyzer/importResolver.ts#L716

---

_Comment by @AlexWaygood on 2025-06-25 10:34_

Right, but I'm terrible at typescript and prefer reading Charlie's rust port 😆

---

_Comment by @MichaReiser on 2025-06-25 10:37_

I mean, the port is type script written in Rust (and it's also not much TypeScript, it's mostly c-like code). Anyway, I don't think these are strong enough reasons to keep the code lingering, especially considering that it has confused contributors because they thought they could build on top of it.

---

_Comment by @AlexWaygood on 2025-06-25 10:40_

It sounds like you feel more strongly than I do — don't let me block you!

>  it has confused contributors because they thought they could build on top of it.

This is a great motivation, and one I haven't got much insight into, because I haven't been engaging with Ruff contributors to nearly the same extent you have over the past few months

---

_Comment by @MichaReiser on 2025-06-25 10:53_

Thank you.

It doesn't come up often but every now and then. There was another instance today (https://github.com/astral-sh/ruff/issues/9103#issuecomment-3002320303) which is why I opened this PR, very well knowing that we decided not to merge similar PRs for the reasons you mentioned before. 

---

_Merged by @MichaReiser on 2025-06-25 10:53_

---

_Closed by @MichaReiser on 2025-06-25 10:53_

---

_Branch deleted on 2025-06-25 10:53_

---
