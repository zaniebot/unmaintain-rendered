```yaml
number: 19390
title: "[`flake8-commas`] Add support for trailing comma checks in type parameter lists (`COM812`, `COM819`)"
type: pull_request
state: merged
author: danparizher
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-18844
created_at: 2025-07-16T22:42:53Z
updated_at: 2025-07-28T23:54:03Z
url: https://github.com/astral-sh/ruff/pull/19390
synced_at: 2026-01-10T17:58:13Z
```

# [`flake8-commas`] Add support for trailing comma checks in type parameter lists (`COM812`, `COM819`)

---

_Pull request opened by @danparizher on 2025-07-16 22:42_

## Summary

Fixes #18844

I'm not too sure if the solution is as simple as the way I implemented it, but I'm curious to see if we are covering all cases correctly here.

---

_Comment by @github-actions[bot] on 2025-07-16 23:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2025-07-18 13:39_

We probably have to gate this behind preview. Did you have a chance to look through the ecosystem results. Do the changes look correct to you?

---

_Label `rule` added by @MichaReiser on 2025-07-18 13:39_

---

_Comment by @danparizher on 2025-07-18 17:29_

Yes, the ecosystem changes look to be correct, from what I can tell. I also gave a shot at implementing the preview, but I'm not sure if I did it correctly. I would appreciate a review!

---

_Review requested from @ntBre by @MichaReiser on 2025-07-23 06:50_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/tokens.rs`:121 on 2025-07-24 16:22_

The `context` has a `LinterSettings` internally. I think it should be okay to add a `pub(crate) fn settings` getter to `LintContext`, basically just like the corresponding `Checker` method here:

https://github.com/astral-sh/ruff/blob/d13228ab856f8cce47b3031cb2b4f2a35401e7eb/crates/ruff_linter/src/checkers/ast/mod.rs#L465-L468

It looks like we could then avoid passing a separate `settings` to `check_tokens` at all, but that would be a bigger refactor we should handle separately.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_commas/rules/trailing_commas.rs`:103 on 2025-07-24 16:25_

nit: we might want a slightly longer example here. This just looks like a list.


```suggestion
    /// Type parameter list, e.g. `def foo[T, U](): ...`
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_commas/rules/trailing_commas.rs`:441 on 2025-07-24 16:37_

Should this still be `ContextType::Subscript`? That seems more consistent with the old code, if I'm following correctly.

I also don't think the preview check is the right conditional to be checking here, at least on its own. I think we'll need to look at more tokens to differentiate between a subscript and a type parameter list. I think the three valid locations for type parameter lists are 
- function definitions
- class definitions
- type alias statements

which is a bit fortuitous because I think that means two tokens of lookbehind should be enough:

```
type Alias[T] = ...
def foo[T](): ...
class C[T]: ...
```

and we already have `prev_prev` as an argument here. Basically I think we want to mimic the `OpeningBracket` case above and check for patterns like `(TokenType::Named, TokenType::Def)` and `(TokenType::Named, TokenType::Class)`, but we'll need to add `TokenType::Class` and `TokenType::Type`, I guess.

---

_@ntBre reviewed on 2025-07-24 16:42_

Thanks! I haven't gone through all of the snapshots yet, but I had a couple of comments on the code in an initial pass.

This is kind of a note to myself for later, but we'll want to double check that the ecosystem check doesn't change on stable for a preview change. It looks like it didn't rerun after you added the preview gate, but hopefully it will run again after any additional commits.

---

_Comment by @danparizher on 2025-07-24 20:53_

It looks like the ecosystem check didn't rerun. Maybe we can manually rerun it?

---

_Review requested from @ntBre by @danparizher on 2025-07-24 20:53_

---

_Comment by @MichaReiser on 2025-07-25 06:50_

The ecosystem results were updated 10h ago. That would suggest that they did run?

---

_Comment by @ntBre on 2025-07-25 13:18_

Maybe there was a queue of ecosystem checks yesterday afternoon. It hadn't updated when I checked after the last commit here yesterday either. I see it now though!

My suggestion might have been slightly misleading about your old preview check. We'll need both the new `match` on two tokens _and_ the preview check. Maybe something like this:

```rust
TokenType::OpeningSquareBracket if is_your_preview_enabled(settings) => match (prev.ty, prev_prev.ty) { // new code
    ...
}
TokenType::OpeningSquareBracket => match prev.ty { // the old code
```

Then hopefully all the ecosystem changes will show up in the `preview` section as expected.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_commas/rules/trailing_commas.rs`:458 on 2025-07-25 21:19_

I think this is wrong. We should use exactly the old code here.
```suggestion
                match prev.ty {
                    TokenType::ClosingBracket | TokenType::Named | TokenType::String => {
                        Context::new(ContextType::Subscript)
                    }
                    _ => Context::new(ContextType::List),
```

And the diff would be smaller if we used a guarded `match` arm. This patch cleared up all of the stable snapshot changes for me locally:

```diff
-        TokenType::OpeningSquareBracket => {
-            if is_trailing_comma_type_params_enabled(settings) {
-                match (prev.ty, prev_prev.ty) {
-                    (TokenType::Named, TokenType::Def) => Context::new(ContextType::TypeParameters),
-                    (TokenType::Named, TokenType::Class) => {
-                        Context::new(ContextType::TypeParameters)
-                    }
-                    (TokenType::Named, TokenType::Type) => {
-                        Context::new(ContextType::TypeParameters)
-                    }
-                    (TokenType::Named | TokenType::String, _) => Context::new(ContextType::List),
-                    (TokenType::ClosingBracket, _) => Context::new(ContextType::Subscript),
-                    _ => Context::new(ContextType::List),
-                }
-            } else {
-                match prev.ty {
-                    TokenType::Named => Context::new(ContextType::List),
-                    TokenType::ClosingBracket => Context::new(ContextType::Subscript),
-                    _ => Context::new(ContextType::List),
+        TokenType::OpeningSquareBracket if is_trailing_comma_type_params_enabled(settings) => {
+            match (prev.ty, prev_prev.ty) {
+                (TokenType::Named, TokenType::Def)
+                | (TokenType::Named, TokenType::Class)
+                | (TokenType::Named, TokenType::Type) => Context::new(ContextType::TypeParameters),
+                (TokenType::ClosingBracket | TokenType::Named | TokenType::String, _) => {
+                    Context::new(ContextType::Subscript)
                 }
+                _ => Context::new(ContextType::List),
             }
         }
+        TokenType::OpeningSquareBracket => match prev.ty {
+            TokenType::ClosingBracket | TokenType::Named | TokenType::String => {
+                Context::new(ContextType::Subscript)
+            }
+            _ => Context::new(ContextType::List),
+        },
```



---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_commas/rules/trailing_commas.rs`:347 on 2025-07-25 21:20_

I don't think we need the preview check here. The only way we can get a `ContextType::TypeParameters` is if the other preview check was true.


```suggestion
            ContextType::TypeParameters => true,
```

---

_@ntBre reviewed on 2025-07-25 21:22_

---

_Comment by @danparizher on 2025-07-25 21:51_

Appreciate the suggestions, I think I was confusing myself with this part 😆

---

_Review requested from @ntBre by @danparizher on 2025-07-25 22:34_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/tokens.rs`:121 on 2025-07-28 14:20_

I'll mark this resolved. I'll follow up here on the bigger `check_tokens` refactor.

---

_@ntBre reviewed on 2025-07-28 14:35_

Thanks. I think you may have missed a couple of the key parts of my patch suggestion. There were still a lot of unexpected differences between the preview and non-preview snapshots, which I saw locally by diffing them. Hopefully we'll get #19351 at some point and this will be easier to see directly in PRs.

I made a couple of tweaks and then confirmed that

```shell
diff crates/ruff_linter/src/rules/flake8_commas/snapshots/ruff_linter__rules__flake8_commas__tests__COM81_syntax_error.py.snap crates/ruff_linter/src/rules/flake8_commas/snapshots/ruff_linter__rules__flake8_commas__tests__preview__COM81_syntax_error.py.snap
```

was totally empty and that

```shell
diff crates/ruff_linter/src/rules/flake8_commas/snapshots/ruff_linter__rules__flake8_commas__tests__COM81.py.snap crates/ruff_linter/src/rules/flake8_commas/snapshots/ruff_linter__rules__flake8_commas__tests__preview__COM81.py.snap
```

only showed modifications at the end of the file, where the new cases were added.

I think that will also cut down on the ecosystem changes. I'll double check that and then merge this.

---

_Renamed from "[`flake8_commas`] Add support for trailing comma checks in type parameter lists for COM812 and COM819" to "[`flake8-commas`] Add support for trailing comma checks in type parameter lists (`COM812`,`COM819`)" by @ntBre on 2025-07-28 14:36_

---

_Comment by @ntBre on 2025-07-28 14:52_

That actually entirely cleared the ecosystem changes :sweat_smile: I hadn't noticed before that they were all related to the slice change, not actual type parameters, but that's the case for all the ones I clicked through just now.

---

_Label `preview` added by @ntBre on 2025-07-28 14:52_

---

_Merged by @ntBre on 2025-07-28 14:53_

---

_Closed by @ntBre on 2025-07-28 14:53_

---

_Branch deleted on 2025-07-28 15:20_

---

_Renamed from "[`flake8-commas`] Add support for trailing comma checks in type parameter lists (`COM812`,`COM819`)" to "[`flake8-commas`] Add support for trailing comma checks in type parameter lists (`COM812`, `COM819`)" by @ntBre on 2025-07-28 23:54_

---
