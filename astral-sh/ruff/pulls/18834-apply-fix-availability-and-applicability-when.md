```yaml
number: 18834
title: "Apply fix availability and applicability when adding to `DiagnosticGuard` and remove `NoqaCode::rule`"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - diagnostics
assignees: []
merged: true
base: main
head: brent/fix-availability
created_at: 2025-06-20T20:26:35Z
updated_at: 2025-06-24T14:08:38Z
url: https://github.com/astral-sh/ruff/pull/18834
synced_at: 2026-01-10T18:39:09Z
```

# Apply fix availability and applicability when adding to `DiagnosticGuard` and remove `NoqaCode::rule`

---

_Pull request opened by @ntBre on 2025-06-20 20:26_

## Summary

This PR removes the last two places we were using `NoqaCode::rule` in `linter.rs` (see https://github.com/astral-sh/ruff/pull/18391#discussion_r2154637329 and https://github.com/astral-sh/ruff/pull/18391#discussion_r2154649726) by  checking whether fixes are actually desired before adding them to a `DiagnosticGuard`. I implemented this by storing a `Violation`'s `Rule` on the `DiagnosticGuard` so that we could check if it was enabled in the embedded `LinterSettings` when trying to set a fix.

All of the corresponding `set_fix` methods on `OldDiagnostic` were now unused (except in tests where I just set `.fix` directly), so I moved these to the guard instead of keeping both sets.

The very last place where we were using `NoqaCode::rule` was in the cache. I just reverted this to parsing the `Rule` from the name. I had forgotten to update the comment there anyway. Hopefully this doesn't cause too much of a perf hit.

In terms of binary size, we're back down almost to where `main` was two days ago (https://github.com/astral-sh/ruff/pull/18391#discussion_r2155034320):

```
41,559,344 bytes for main 2 days ago
41,669,840 bytes for #18391
41,653,760 bytes for main now (after #18391 merged)
41,602,224 bytes for this branch
```

Only 43 kb up, but that shouldn't all be me this time :)

## Test Plan

Existing tests and benchmarks on this PR


---

_Label `internal` added by @ntBre on 2025-06-20 20:26_

---

_Label `diagnostics` added by @ntBre on 2025-06-20 20:26_

---

_Comment by @github-actions[bot] on 2025-06-20 20:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @ntBre on 2025-06-20 20:45_

---

_Review requested from @MichaReiser by @ntBre on 2025-06-20 20:45_

---

_@MichaReiser reviewed on 2025-06-21 16:05_

---

_Review comment by @MichaReiser on `crates/ruff/src/cache.rs`:445 on 2025-06-21 16:05_

This now seems to be the only reason why we need 	`::strum_macros::EnumString` which requires a fair amount of code just to make this work. 

I think we can get something comparable without going through rule by building an ad-hoc interner by rule id:

```rust
			// Create an index map that maps rule name to index
			let mut rules = indexmap::IndexMap::new();
			
			// ...

			// In filter map, retrieve the index or insert the rule with a new index

                let len = rules.len();
                let rule_index = rules.entry(msg.noqa_code()?).or_insert(len);

                Some((*rule_index, msg))

			// ...

			// Store the rule index on `LintCachedData
			let rules = rules.into_keys().map(|code| code.to_string()).collect();
		 
       Self {
            messages,
            source,
            rules,
            notebook_index,
        }
```

The deserialization can then lookup the rule in `rules` and clone the value. 


This will also allow us to remove the `Serialize` and `Deserialize` derive from `Rule`

(And I think we can remove `Ord`, `PartialOrd`, and `CacheKey` too

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:3252 on 2025-06-21 16:11_

Does this match Ruff's current behavior? Does that mean, we won't even **show** fixes when the fix is disabled. This seems wrong to me.

I'm not suggesting we should necessarily change this in this PR but we should at least open an issue for what I consider a bug. Instead, Ruff should keep the fix (but mark it in some way) and we then skip it when applying automatic fixes (and we don't show it in the LSP although I think even that would be fine).

Which makes me wonder. Should `should_fix == False` resolve to manual applicability?

---

_@MichaReiser approved on 2025-06-21 16:14_

Nice, I think we can remove some more code by not using `Rule` in the cache. 

We should also double check if this exactly captures today's behavior. E.g. does the LSP not show fixes for rules with `should_fix = false`? Do we show the fixes in diagnostics? 

---

_@ntBre reviewed on 2025-06-21 18:10_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:3252 on 2025-06-21 18:10_

Yes, this does seem to match the current behavior in both the [playground](https://play.ruff.rs/a5f9247e-5ada-4670-bd1a-f96eaeb790ab) and in VS Code, with the caveat that I couldn't get a `ruff.toml` like 

```toml
[lint]
unfixable = ["F401"]
```

to work in VS Code, only setting it in my `settings.json` actually applied. This might be a bug too, unless I was doing something wrong.

I think the LSP filters out fixes with display-only applicability here:

https://github.com/astral-sh/ruff/blob/b2b65e72eba8222f157fe3417ff7bf3ef034d122/crates/ruff_server/src/lint.rs#L247

But if we change that to `DisplayOnly` and change the code here to 

```rust
        if !self.context.rules.should_fix(self.rule) {
            self.fix = Some(fix.with_applicability(Applicability::DisplayOnly));
            return;
        }
```

we can show display-only fixes in the editor, even if they're disabled. That definitely makes sense to me.

---

_@ntBre reviewed on 2025-06-21 19:07_

---

_Review comment by @ntBre on `crates/ruff/src/cache.rs`:445 on 2025-06-21 19:07_

What is the type of the `IndexMap` here? I think we still need to get back to the `Rule` because it's needed to reconstruct the `OldDiagnostic` in `OldDiagnostic::lint`.

Another option here could be `Rule::from_code(&msg.noqa_code()?.to_string()).ok()?)`. And then we could store the `Rule` as its `repr(u16)` in the cache. That should allow us to remove all of the same derives? (Although it does add back one derive: `strum_macros::FromRepr` for the conversion in the other direction)

<details><summary>Patch</summary>

```diff
diff --git a/crates/ruff/src/cache.rs b/crates/ruff/src/cache.rs
index fdaf6b4a7..14710027a 100644
--- a/crates/ruff/src/cache.rs
+++ b/crates/ruff/src/cache.rs
@@ -356,7 +356,8 @@ impl FileCache {
                             msg.parent,
                             file.clone(),
                             msg.noqa_offset,
-                            msg.rule,
+                            Rule::from_repr(msg.rule)
+                                .expect("Expected a valid rule repr in the cache"),
                         )
                     })
                     .collect()
@@ -442,7 +443,7 @@ impl LintCacheData {
             // Parse the kebab-case rule name into a `Rule`. This will fail for syntax errors, so
             // this also serves to filter them out, but we shouldn't be caching files with syntax
             // errors anyway.
-            .filter_map(|msg| Some((msg.name().parse().ok()?, msg)))
+            .filter_map(|msg| Some((Rule::from_code(&msg.noqa_code()?.to_string()).ok()?, msg)))
             .map(|(rule, msg)| {
                 // Make sure that all message use the same source file.
                 assert_eq!(
@@ -451,7 +452,7 @@ impl LintCacheData {
                     "message uses a different source file"
                 );
                 CacheMessage {
-                    rule,
+                    rule: rule as u16,
                     body: msg.body().to_string(),
                     suggestion: msg.suggestion().map(ToString::to_string),
                     range: msg.range(),
@@ -475,7 +476,7 @@ impl LintCacheData {
 pub(super) struct CacheMessage {
     /// The rule for the cached diagnostic.
     #[bincode(with_serde)]
-    rule: Rule,
+    rule: u16,
     /// The message body to display to the user, to explain the diagnostic.
     body: String,
     /// The message to display to the user, to explain the suggested fix.
diff --git a/crates/ruff_macros/src/map_codes.rs b/crates/ruff_macros/src/map_codes.rs
index 39993af6a..6f9b0d043 100644
--- a/crates/ruff_macros/src/map_codes.rs
+++ b/crates/ruff_macros/src/map_codes.rs
@@ -433,13 +433,8 @@ fn register_rules<'a>(input: impl Iterator<Item = &'a Rule>) -> TokenStream {
             Copy,
             Clone,
             Hash,
-            PartialOrd,
-            Ord,
-            ::ruff_macros::CacheKey,
             ::strum_macros::IntoStaticStr,
-            ::strum_macros::EnumString,
-            ::serde::Serialize,
-            ::serde::Deserialize,
+            ::strum_macros::FromRepr,
         )]
         #[repr(u16)]
         #[strum(serialize_all = "kebab-case")]
```

</details>


---

_@MichaReiser reviewed on 2025-06-21 19:27_

---

_Review comment by @MichaReiser on `crates/ruff/src/cache.rs`:445 on 2025-06-21 19:27_

Hmm I see. It's because of the `&'static str` in `LintId`. I don't think what I suggested then makes sense. But this does feel unfortunate. It particularly annoys me that `filter_map` seems a bit like a footgun. It's very easy to filter out too many diagnostics (e.g. if Ruff ever starts using any other `DiagnosticId` other than syntax error). It even becomes more fragile when we make `noqa_code` (secondary code) an `Option` on `ruff_db::Diagnostic`. 

I think the options are:

* Make `LintName` use a `String`. Which doesn't feel great as well
* implement a serialize and deserialize function (in ruff) for `LintId`. That's probably the way to go but I'm okay to defer this to another PR. 


It's unfortunate that we can't remove all `FromRepr` calls or is there a way to get to the rule from the `LintName`? I think that would be better if possible

---

_Review comment by @ntBre on `crates/ruff/src/cache.rs`:445 on 2025-06-23 14:35_

I think if we kept only `EnumString`, we could serialize the `LintName` as a `String` here and then parse back to a rule when deserializing. That should eliminate all of the derives except `EnumString`. Would that be any better?

Then we could also replace this `filter_map` with a `filter(|msg| !msg.is_syntax_error())`, I think. Although that still doesn't really address using other `DiagnosticId`s since I don't think those could be parsed back to `Rule`s either.

But yeah, we're just using the `Rule` to retrieve the `&'static str` lint name. Otherwise we could just (de)serialize the lint name and the `Option<NoqaCode>` (well, probably as an `Option<String>`) directly.

How are we planning to handle this caching in ty? It seems like there would be issues with the `&'static str` there too.

---

_@ntBre reviewed on 2025-06-23 14:35_

---

_@MichaReiser reviewed on 2025-06-23 14:38_

---

_Review comment by @MichaReiser on `crates/ruff/src/cache.rs`:445 on 2025-06-23 14:38_

> I think if we kept only EnumString, we could serialize the LintName as a String here and then parse back to a rule when deserializing. That should eliminate all of the derives except EnumString. Would that be any better?

I'm not sure what you mean by `EnumString`.

> How are we planning to handle this caching in ty? It seems like there would be issues with the &'static str there too.

Yes, I think we would have the same issue. I think going through `Rule`s here is probably fine (that's what I would do in ty). 

---

_@MichaReiser reviewed on 2025-06-23 14:39_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:3252 on 2025-06-23 14:39_

Let's create a follow up issue for this. I think it requires some design work on how we want to handle them in the LSP.

---

_@ntBre reviewed on 2025-06-23 14:46_

---

_Review comment by @ntBre on `crates/ruff/src/cache.rs`:445 on 2025-06-23 14:46_

I mean `::strum_macros::EnumString`, which I thought spawned this thread. We're currently using `EnumString` to parse a rule name to a `Rule` when _serializing_. But if the goal is to remove several derive macros, we could instead serialize the string and call `name.parse().unwrap()` when _deserializing_. That would remove the need for all of these macros[^1]:

https://github.com/astral-sh/ruff/blob/291413b126ad44adc60fe3374ab5020d5659b92c/crates/ruff_macros/src/map_codes.rs#L456-L462

_except_ `EnumString`, which was the original target. I may have lost the thread here, though, if you meant something else entirely.

[^1]: We also still need `IntoStaticStr`, which we use for the `Display` implementation, but that's a bit beside the point.

---

_@MichaReiser reviewed on 2025-06-23 14:53_

---

_Review comment by @MichaReiser on `crates/ruff/src/cache.rs`:445 on 2025-06-23 14:53_

Ahh, sorry. That's on me. My main goal was to avoid needing to go back to `Rule`. 

I don't think I feel very strongly about this. Removing `EnumString ` would definitely be nice and I could see other ways to avoid the `name.parse()` overhead (... bring back my interner ;)). 

---

_@ntBre reviewed on 2025-06-23 14:58_

---

_Review comment by @ntBre on `crates/ruff/src/cache.rs`:445 on 2025-06-23 14:58_

Sounds good, I think I'll merge this for now then. I think there will definitely be an interner in our future! :smile: 

---

_Merged by @ntBre on 2025-06-24 14:08_

---

_Closed by @ntBre on 2025-06-24 14:08_

---

_Branch deleted on 2025-06-24 14:08_

---
