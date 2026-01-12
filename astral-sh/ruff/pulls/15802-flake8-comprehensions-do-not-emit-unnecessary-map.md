```yaml
number: 15802
title: "[`flake8-comprehensions`] Do not emit `unnecessary-map` diagnostic when lambda has different arity (`C417`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
assignees: []
merged: true
base: main
head: C417
created_at: 2025-01-29T01:14:09Z
updated_at: 2025-01-29T19:00:40Z
url: https://github.com/astral-sh/ruff/pull/15802
synced_at: 2026-01-12T15:55:52Z
```

# [`flake8-comprehensions`] Do not emit `unnecessary-map` diagnostic when lambda has different arity (`C417`)

---

_@InSyncWithFoo_

## Summary

Resolves #15796.

The cases discussed in the aforementioned issue are no longer reported.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-29 01:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2025-01-29 12:07_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_comprehensions/C417.py`:22 on 2025-01-29 12:07_

this one actually doesn't raise an exception at runtime (possibly because multiple iterables are being passed in after the function?). It would be great if we could keep emitting a diagnostic for this one.

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-01-29 12:07_

---

_@InSyncWithFoo reviewed on 2025-01-29 13:53_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/resources/test/fixtures/flake8_comprehensions/C417.py`:22 on 2025-01-29 13:53_

`map()` calls with multiple iterables aren't currently reported. Enhancing the rule is perhaps out of scope for this PR. I filed #15809 instead.

---

_@AlexWaygood reviewed on 2025-01-29 18:09_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_comprehensions/C417.py`:22 on 2025-01-29 18:09_

I see. In that case, I probably wouldn't have moved this line in the fixture, since it's already under a line that says `# false negatives`. But it's not the biggest issue in the world.

---

_@AlexWaygood reviewed on 2025-01-29 18:11_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_comprehensions/C417.py`:22 on 2025-01-29 18:11_

Actually, could you possibly move it back? It doesn't belong under the `# https://github.com/astral-sh/ruff/issues/15796` comment you've moved it under, because that issue is specifically about `map()` calls that would raise exceptions which Ruff erroneously replaces with code that doesn't. But that doesn't apply to this line.

---

_Comment by @AlexWaygood on 2025-01-29 18:14_

You can do this to fix the Clippy error:

```diff
diff --git a/crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_map.rs b/crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_map.rs
index 2b21e1806..8cb0a08f3 100644
--- a/crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_map.rs
+++ b/crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_map.rs
@@ -111,7 +111,7 @@ pub(crate) fn unnecessary_map(
                 return;
             }
 
-            if !lambda_has_expected_arity(parameters.as_ref(), body) {
+            if !lambda_has_expected_arity(parameters.as_deref(), body) {
                 return;
             }
         }
@@ -146,7 +146,7 @@ pub(crate) fn unnecessary_map(
                 return;
             };
 
-            if !lambda_has_expected_arity(parameters.as_ref(), body) {
+            if !lambda_has_expected_arity(parameters.as_deref(), body) {
                 return;
             }
         }
@@ -191,7 +191,7 @@ pub(crate) fn unnecessary_map(
                 return;
             }
 
-            if !lambda_has_expected_arity(parameters.as_ref(), body) {
+            if !lambda_has_expected_arity(parameters.as_deref(), body) {
                 return;
             }
         }
@@ -216,11 +216,10 @@ pub(crate) fn unnecessary_map(
 /// * It has exactly one parameter
 /// * That parameter is not variadic
 /// * That parameter does not have a default value
-fn lambda_has_expected_arity(parameters: Option<&Box<Parameters>>, body: &Expr) -> bool {
+fn lambda_has_expected_arity(parameters: Option<&Parameters>, body: &Expr) -> bool {
     let Some(parameters) = parameters else {
         return false;
     };
-    let parameters = parameters.as_ref();
 
     let [parameter] = &parameters.args[..] else {
         return false;
```

---

_@AlexWaygood approved on 2025-01-29 18:43_

Thanks!

---

_Renamed from "[`flake8-comprehensions`] Do not emit diagnostic when lambda has different arity (`C417`)" to "[`flake8-comprehensions`] Do not emit `unnecessary-map` diagnostic when lambda has different arity (`C417`)" by @AlexWaygood on 2025-01-29 18:44_

---

_Label `bug` added by @AlexWaygood on 2025-01-29 18:44_

---

_Merged by @AlexWaygood on 2025-01-29 18:45_

---

_Closed by @AlexWaygood on 2025-01-29 18:45_

---

_Branch deleted on 2025-01-29 19:00_

---
