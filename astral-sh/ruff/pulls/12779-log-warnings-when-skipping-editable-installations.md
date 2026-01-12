```yaml
number: 12779
title: Log warnings when skipping editable installations
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: warn-about-skipped-pth-files
created_at: 2024-08-09T10:10:01Z
updated_at: 2024-08-09T14:29:46Z
url: https://github.com/astral-sh/ruff/pull/12779
synced_at: 2026-01-12T15:55:42Z
```

# Log warnings when skipping editable installations

---

_@MichaReiser_

## Summary

This PR adds warning messages when skipping editable installations instead of skipping them silently.

## Test Plan

Not sure how to test this best without spending too much time on it. 


---

_Review requested from @carljm by @MichaReiser on 2024-08-09 10:10_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-08-09 10:10_

---

_Label `red-knot` added by @MichaReiser on 2024-08-09 10:10_

---

_Comment by @github-actions[bot] on 2024-08-09 10:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-08-09 10:28_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:161 on 2024-08-09 10:28_

We need to get the path from `search_path` because it is the canonicalized path (whereas `path` isn't). 

This is covered by the symlink test

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:370 on 2024-08-09 10:32_

Hmm, I wonder if the name of this method should change now. It no longer yields fully validated editable-installation search paths; some additional validation is now needed for each path it yields. Maybe `editable_install_candidates`? Not sure...

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:161 on 2024-08-09 10:36_

All the added `.as_system_path().unwrap()` calls are kind of a shame though. Could we do the `files.try_add_root()` call inside the `SearchPath::extra()` call (as soon as we've canonicalised the system path, but before we've wrapped it in the enum)?

---

_@AlexWaygood reviewed on 2024-08-09 10:36_

---

_@MichaReiser reviewed on 2024-08-09 11:41_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:161 on 2024-08-09 11:41_

A side effect like adding a path to `files.try_add_root` feels off to me in the `SearchPath` constructor. 

But agree, it's a bit an annoyance. The only other solution I can think of is to separate the validation of a system-search path from its construction. But that feels more involved

---

_@MichaReiser reviewed on 2024-08-09 11:44_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:370 on 2024-08-09 11:44_

Feels too long to me. The terminology used by Python is to call these `items`

> A path configuration file is a file whose name has the form name.pth and exists in one of the four directories mentioned above; its contents are additional items (one per line) to be added to sys.path. 

---

_@AlexWaygood reviewed on 2024-08-09 11:45_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:370 on 2024-08-09 11:45_

just calling the method `items()` works for me. It's the items of a `.pth` file

---

_@AlexWaygood reviewed on 2024-08-09 11:48_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:161 on 2024-08-09 11:48_

> A side effect like adding a path to `files.try_add_root` feels off to me in the `SearchPath` constructor.

To me it feels correct, actually. The constructor creates a validated, normalized search path; once a `SearchPath` instance has been created, that's meant to represent that it's been fully validated and can be used without issue by other modules. But one of the necessary steps to get to that point is to register the search path as a `FileRoot`; so to me it makes sense to have it as part of the constructor

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:161 on 2024-08-09 12:30_

I still think it couples too many different things together. A search path should be allowed to exist without having a `FileRoot`. They don't strictly belong together. It's only site packages where we need one, but not because the site-packages search path would be incomplete without one. 

I think my preferred solution would be to actually decouple the existence of a `SearchPath` from its validation but that's a longer discussion that we probably don't agree with. 

I would prefer to keep it as is for now

---

_@MichaReiser reviewed on 2024-08-09 12:30_

---

_@AlexWaygood approved on 2024-08-09 12:35_

---

_@AlexWaygood reviewed on 2024-08-09 12:35_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:161 on 2024-08-09 12:35_

fair enough

---

_@MichaReiser reviewed on 2024-08-09 12:38_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:161 on 2024-08-09 12:38_

I can address your concern of too many unwraps by moving the canonicalize call out of the constructor. Would you prefer this? 

---

_@AlexWaygood reviewed on 2024-08-09 12:41_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:161 on 2024-08-09 12:41_

> I can address your concern of too many unwraps by moving the canonicalize call out of the constructor. Would you prefer this?

Yeah... I think so... but we should then add a note to the docs of the constructor that say that it's expected that the path has already been canonicalised before it's passed to the constructor

---

_@MichaReiser reviewed on 2024-08-09 12:53_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:161 on 2024-08-09 12:53_

Okay. I'll do that but I think I'm going to do it as a follow up PR so that I have fewer merge conflicts.

---

_@AlexWaygood reviewed on 2024-08-09 12:55_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:161 on 2024-08-09 12:55_

sure

---

_Merged by @MichaReiser on 2024-08-09 14:29_

---

_Closed by @MichaReiser on 2024-08-09 14:29_

---

_Branch deleted on 2024-08-09 14:29_

---
