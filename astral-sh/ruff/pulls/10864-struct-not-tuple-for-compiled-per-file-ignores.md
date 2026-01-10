```yaml
number: 10864
title: Struct not tuple for compiled per-file ignores
type: pull_request
state: merged
author: carljm
labels:
  - internal
assignees: []
merged: true
base: main
head: cjm/structify
created_at: 2024-04-10T20:31:19Z
updated_at: 2024-04-11T19:47:58Z
url: https://github.com/astral-sh/ruff/pull/10864
synced_at: 2026-01-10T22:37:01Z
```

# Struct not tuple for compiled per-file ignores

---

_Pull request opened by @carljm on 2024-04-10 20:31_

## Summary

Code cleanup for per-file ignores; use a struct instead of a tuple.

Named the structs for individual ignores and the list of ignores `CompiledPerFileIgnore` and `CompiledPerFileIgnoreList`. Name choice is because we already have a `PerFileIgnore` struct for a pre-compiled-matchers form of the config. Name bikeshedding welcome.

## Test Plan

Refactor, should not change behavior; existing tests pass.


---

_Label `internal` added by @carljm on 2024-04-10 20:31_

---

_Comment by @github-actions[bot] on 2024-04-10 20:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @BurntSushi by @charliermarsh on 2024-04-10 23:18_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/settings/types.rs`:622 on 2024-04-11 06:19_

I think we have a custom macro for displaying settings. 

https://github.com/astral-sh/ruff/blob/317d2e4c757f175e275eeb077f4d2045c8797956/crates/ruff_workspace/src/settings.rs#L57-L76

@snowsignal should we use the `display_settings` macro here?

---

_@MichaReiser approved on 2024-04-11 06:19_

I like it :)

---

_@BurntSushi approved on 2024-04-11 11:36_

LGTM!

---

_@snowsignal reviewed on 2024-04-11 15:13_

---

_Review comment by @snowsignal on `crates/ruff_linter/src/settings/types.rs`:622 on 2024-04-11 15:13_

Yep, this would be a good use case for it 😄 

---

_@carljm reviewed on 2024-04-11 17:51_

---

_Review comment by @carljm on `crates/ruff_linter/src/settings/types.rs`:622 on 2024-04-11 17:51_

Paired with Alex to implement this!

---

_@carljm reviewed on 2024-04-11 17:52_

---

_Review comment by @carljm on `crates/ruff_linter/src/settings/types.rs`:622 on 2024-04-11 17:52_

It did require adding a new format/type whatever of "globmatcher" to the `display_settings!` macro.

---

_Comment by @carljm on 2024-04-11 18:23_

@MichaReiser @snowsignal requesting another review because I would like to make sure that the change we made to `display_settings!` (to give it capability to display a GlobMatcher reasonably) looks OK.

The previous `Display` implementation here was actually pretty useless, it used `{:#?}` for a GlobMatcher which gives a massive output with all the internal structure of the GlobMatcher; suitable for debugging a `GlobMatcher` but not suitable for `Display` of the per-file-ignore settings at all. Given that nobody noticed that, I'm guessing we may never really use Display on these anywhere?

But anyway, the new version here is much better / more readable (for Display purposes), it just shows the actual file pattern of the `GlobMatcher`.

---

_Review requested from @MichaReiser by @carljm on 2024-04-11 18:23_

---

_Review requested from @snowsignal by @carljm on 2024-04-11 18:23_

---

_Review comment by @snowsignal on `crates/ruff_linter/src/settings/mod.rs`:134 on 2024-04-11 18:28_

Nit: I think this fits better with the naming scheme used elsewhere in the macro.
```suggestion
    (@field $fmt:ident, $prefix:ident, $settings:ident.$field:ident | glob) => {
```

---

_@snowsignal approved on 2024-04-11 18:29_

Looks good to me! Thank you 😁 

---

_Review comment by @carljm on `crates/ruff_linter/src/settings/mod.rs`:134 on 2024-04-11 19:22_

Happy to change it, but does it shift your preference at all to note that globset has both `GlobMatcher` and `Glob` as distinct types, and this added case only works for a setting that is a `GlobMatcher`? For a `Glob` this new case won't work, but you can just use the regular default display with good results. (That's why we named the new case `globmatcher`.)

---

_@carljm reviewed on 2024-04-11 19:22_

---

_@snowsignal reviewed on 2024-04-11 19:46_

---

_Review comment by @snowsignal on `crates/ruff_linter/src/settings/mod.rs`:134 on 2024-04-11 19:46_

Okay, I can see why using the full type name makes sense. I'm not too opinionated on the naming scheme (it's not that consistent to begin with, anyway) so we can keep using that. Thanks for explaining 👍 

---

_Merged by @carljm on 2024-04-11 19:47_

---

_Closed by @carljm on 2024-04-11 19:47_

---

_Branch deleted on 2024-04-11 19:47_

---
