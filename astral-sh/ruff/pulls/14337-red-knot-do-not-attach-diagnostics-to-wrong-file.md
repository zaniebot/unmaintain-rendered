```yaml
number: 14337
title: "[red-knot] Do not attach diagnostics to wrong file"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-14334
created_at: 2024-11-14T12:43:33Z
updated_at: 2024-11-14T14:39:52Z
url: https://github.com/astral-sh/ruff/pull/14337
synced_at: 2026-01-12T15:55:47Z
```

# [red-knot] Do not attach diagnostics to wrong file

---

_@sharkdp_

## Summary

Avoid attaching diagnostics to the wrong file. See related issue for details.

Closes #14334

## Test Plan

New regression test.

---

_Label `red-knot` added by @sharkdp on 2024-11-14 12:43_

---

_Review requested from @carljm by @sharkdp on 2024-11-14 12:43_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-14 12:43_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-14 12:43_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:337 on 2024-11-14 12:44_

I can try to unify this with `add_if_expression_in_same_file` into a single `add_if_in_same_file` (using yet another trait?), but I first wanted to check if this approach goes in the right direction.

---

_@sharkdp reviewed on 2024-11-14 12:44_

---

_@sharkdp reviewed on 2024-11-14 12:46_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:482 on 2024-11-14 12:46_

Other diagnostics might need the same "treatment". Again, I first wanted to see if this goes in the right direction. Maybe we want to store a reference to `SemanticIndexBuilder` in the `DiagnosticsBuilder` and check this for every diagnostic that we add?

---

_@AlexWaygood reviewed on 2024-11-14 12:56_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/regression/14334_diagnostics_in_wrong_file.md`:1 on 2024-11-14 12:56_

I can also make this test pass just by applying this diff to `main`, without any of the other changes you make in this PR:

```diff
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -471,7 +471,8 @@ impl<'db> TypeInferenceBuilder<'db> {
             .declarations
             .values()
             .filter_map(|ty| ty.into_class_literal())
-            .map(|class_ty| class_ty.class);
+            .map(|class_ty| class_ty.class)
+            .filter(|class| class.file(self.db) == self.file);
```

...and I think that might be a better fix. For most of the diagnostics we emit, I think we can be confident that the diagnostic applies to the file we're currently in, because we emit the diagnostic while visiting a specific AST node from that file. The diagnostics emitted from `check_class_definitions()` are unusual, in that they're emitted after the AST of the whole module has been visited. (That's necessary because of the fact that inference of class bases is sometimes deferred.)

---

_Comment by @github-actions[bot] on 2024-11-14 12:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-11-14 12:59_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/regression/14334_diagnostics_in_wrong_file.md`:1 on 2024-11-14 12:59_

I'm in favor of a more local fix, considering that class inheritance is the odd one. 

---

_@AlexWaygood reviewed on 2024-11-14 13:00_

Great catch!! I think your idea in https://github.com/astral-sh/ruff/issues/14334#issue-2658241214 of adding some assertions (or making this kind of thing impossible to happen in the first place) would be a great improvement. It didn't even occur to me that this iterator would also iterate over class definitions that originated from other files as well here, though now it seems obvious.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:480 on 2024-11-14 13:00_

All diagnostics here should be guaranteed to be from the same file because `class.node` is from the current scope. If it's not, then we should not access the node in the first place. Accessing nodes across module boundaries in type-inference is bad because it results in larger invalidation.

---

_@MichaReiser reviewed on 2024-11-14 13:00_

---

_@sharkdp reviewed on 2024-11-14 13:10_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:480 on 2024-11-14 13:10_

Hm. You are probably right. I added it everywhere because the original error appeared with an example file without any class definitions at all:
```
from subprocess import Popen
```
Or do we consider the import of `Popen` to be a class definition in this file?

---

_@AlexWaygood reviewed on 2024-11-14 13:12_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:480 on 2024-11-14 13:12_

> Or do we consider the import of `Popen` to be a class definition in this file?

The import of `Popen` constitutes a `Definition` and `Declaration` of the `Popen` symbol in this file, and the type of that `Declaration` is a `ClassLiteralType`...!

---

_@AlexWaygood reviewed on 2024-11-14 13:14_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/regression/14334_diagnostics_in_wrong_file.md`:1 on 2024-11-14 13:14_

> I'm in favor of a more local fix, considering that class inheritance is the odd one.

I'm not sure if you're agreeing with me or disagreeing with me 😄 Do you mean "more local than David's current PR" or "more local than my alternative fix"?

---

_@sharkdp reviewed on 2024-11-14 13:19_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:480 on 2024-11-14 13:19_

Ah, okay. Thanks! ~~I reverted the check for the local classes.~~ Wait, no. I need to think …

---

_@AlexWaygood reviewed on 2024-11-14 13:29_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:480 on 2024-11-14 13:29_

I'm still not sure we should be looking at any classes that originate from other modules in this `class_definitions` iterator (https://github.com/astral-sh/ruff/pull/14337#discussion_r1842177196)

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:480 on 2024-11-14 13:32_

No, I think we need that check here. I added an additional test to check for this. If we don't check this here, we will get an additional diagnostic at the line of the import (with a wrong `range`).

---

_@sharkdp reviewed on 2024-11-14 13:32_

---

_@sharkdp reviewed on 2024-11-14 13:34_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/regression/14334_diagnostics_in_wrong_file.md`:1 on 2024-11-14 13:34_

> considering that class inheritance is the odd one

No. It's not because of class inheritance. It also appears for the other error kinds. My regression test was maybe a bit misleading (that derived class is not even needed, the mistake happens at the import statement). I modified it the test now and added a new one.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:480 on 2024-11-14 13:42_

I think the problem is the `self.types.declarations.values()`. We only want to iterate over classes that were declared in this file. Instead, it iterates over all declarations that have a class type, that includes imports and possibly others. 

We should adopt the filter above to filter out classes from other scopes AND only collect unique literals. 

---

_@MichaReiser reviewed on 2024-11-14 13:42_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:480 on 2024-11-14 13:45_

Or a better way might be to filter based on the `declaration`s key to assert that it is a class definition

---

_@MichaReiser reviewed on 2024-11-14 13:45_

---

_@MichaReiser reviewed on 2024-11-14 13:48_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:480 on 2024-11-14 13:48_

Something like this

```
let class_definitions = self
            .types
            .declarations
            .iter()
            .filter_map(|(definition, ty)| {
                if matches!(definition.kind(self.db), DefinitionKind::Class(_)) {
                    ty.into_class_literal()
                } else {
                    None
                }
            })
            .map(|class_ty| class_ty.class);
```

---

_@MichaReiser reviewed on 2024-11-14 13:52_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:480 on 2024-11-14 13:52_

Sorry for keeping commenting. A related improvement would be that the iterator also returns the `node` so that we can replace the `class.node` calls (they're not expensive but getting them directly from the definition makes it more explicit that they are guaranteed to be from the same scope)

---

_@sharkdp reviewed on 2024-11-14 13:58_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/regression/14334_diagnostics_in_wrong_file.md`:1 on 2024-11-14 13:58_

Ok, the new approach is similar to what you proposed, @AlexWaygood. I removed all of my `if_same_file` functionality for now. Adding that assertion somewhere would probably still be a good idea.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:480 on 2024-11-14 14:04_

> Something like this

Saw this too late, but came up with the exact same code. Except for a name difference (now changed to yours).





> A related improvement would be that the iterator also returns the `node`

Done.

---

_@sharkdp reviewed on 2024-11-14 14:04_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:481 on 2024-11-14 14:07_

Sorry, what I meant is this

```suggestion
            .iter()
            .filter_map(|(definition, ty)| {
                // Filter out class literals that result from imports
                if let DefinitionKind::Class(class) = definition.kind(self.db) {
                    ty.into_class_literal().map(|ty| (class, ty.class))
                } else {
                    None
                }
            });
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:483 on 2024-11-14 14:08_

to me it felt cleaner when we only iterated over the `Class`es, and retrieved the AST node for the class only if there was actually an error associated with that class. But 🤷 no strong opinion, if @MichaReiser prefers it this way

---

_@AlexWaygood approved on 2024-11-14 14:08_

Thanks!

---

_@MichaReiser approved on 2024-11-14 14:09_

Thanks, nice find!

It would be nice if we could add an assertion to the diagnostic builder that enforces this but it isn't clear to me how, at least if it should work for arbitrary nodes

---

_@MichaReiser reviewed on 2024-11-14 14:10_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:483 on 2024-11-14 14:10_

I hope my latest suggestion makes it more obvious why I prefer it this way

---

_@AlexWaygood reviewed on 2024-11-14 14:11_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:483 on 2024-11-14 14:11_

it does, thanks!!

---

_@sharkdp reviewed on 2024-11-14 14:25_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:481 on 2024-11-14 14:25_

Ah, okay. Now it makes more sense. I assume you mean `class.node()` instead of `class`?

---

_Merged by @sharkdp on 2024-11-14 14:39_

---

_Closed by @sharkdp on 2024-11-14 14:39_

---

_Branch deleted on 2024-11-14 14:39_

---
