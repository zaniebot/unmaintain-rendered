```yaml
number: 12522
title: "[red-knot] qualify function/class name with module/file"
type: pull_request
state: closed
author: carljm
labels:
  - ty
assignees: []
base: main
head: cjm/display-modname
created_at: 2024-07-26T05:14:43Z
updated_at: 2024-07-26T17:20:24Z
url: https://github.com/astral-sh/ruff/pull/12522
synced_at: 2026-01-12T15:55:41Z
```

# [red-knot] qualify function/class name with module/file

---

_@carljm_

When we display a class or function type, include the module name, not just the class/function name.

If the file in which it's defined is not an importable module (e.g. a script outside the import search paths), use the path to the file in brackets instead of a module name.

---

_Label `red-knot` added by @carljm on 2024-07-26 05:14_

---

_Review requested from @MichaReiser by @carljm on 2024-07-26 05:14_

---

_Review requested from @AlexWaygood by @carljm on 2024-07-26 05:14_

---

_Comment by @github-actions[bot] on 2024-07-26 05:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:63 on 2024-07-26 06:08_

I would prefer to fix the lint warning here instead, or to not make the type public (either way is fine)

---

_@MichaReiser reviewed on 2024-07-26 06:13_

Is the main motivation for this change to support the assertions in tests? 

If so, then I don't think `Display` is the right approach. 

I just tested pylance and it doesn't always show the full qualified name. Which makes sense, it would be way to verbose for types imported from the same module, but it also doesn't do so for types from other modules.

---

_@carljm reviewed on 2024-07-26 13:59_

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/resolver.rs`:63 on 2024-07-26 13:59_

This is a bug with making Salsa tracked functions `pub`. Even if the tracked `pub` function is both reachable and used (which it is in this PR), Salsa also creates a struct by the same name, which is not reachable, so whenever you make a Salsa tracked function `pub` and expose it with `pub use`, you get this spurious lint warning. I don't know any way to fix it, unless we make this whole submodule `pub`, or move the function out to `lib.rs`.

---

_@MichaReiser reviewed on 2024-07-26 14:08_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:63 on 2024-07-26 14:08_

Ahh, a TODO would be helpful then.

I know how to fix it, upgrade salsa :laughing: 

---

_Comment by @carljm on 2024-07-26 15:03_

The main motivation for the change is not tests, it's just the fact that always displaying un-qualified names is ambiguous and potentially confusing. But I think you're right that we may want a smarter algorithm for deciding when to fully qualify names and when not to (e.g. if we are displaying an error message in a file, we probably don't want to fully qualify names in that same file).

I still think "fully unambiguous" is the best default starting point, and we can try to optimize from there.

---

_Comment by @carljm on 2024-07-26 15:27_

Hmm, it does look like both pyright and mypy just never qualify type names, and if you have ambiguous names it's your own problem. Maybe that's strong enough evidence that we don't need this. I'm curious what @AlexWaygood thinks, but I'll close this for now.

---

_Closed by @carljm on 2024-07-26 15:27_

---

_Comment by @carljm on 2024-07-26 15:41_

It looks like both mypy and pyright have logic to use fully-qualified names only if there are actually two ambiguously-named types used in the same module. This makes sense as an approach, but it's something we can tackle later.

---

_Comment by @AlexWaygood on 2024-07-26 15:46_

I think you're right that mypy uses the shorter name where possible in error messages, but note that if you call `reveal_type()` on an expression, it always gives you the fully qualified name of the type. That sorta feels annoyingly inconsistent to me, though I can see the logic in it — you want to keep error messages reasonably concise if you can, but if a user is calling `reveal_type` on an expression, they're likely debugging something, so you want to be 100% unambiguous in the information you're providing

---

_Comment by @carljm on 2024-07-26 15:48_

I don't think there's any pressing need to merge this change right now, so I'm putting the larger issue on the roadmap as something to tackle later.

---

_Comment by @MichaReiser on 2024-07-26 17:20_

Seems reasonable. We probably want something more than `Display` in that case. An ide probably wants to show both, but in different places. 

---
