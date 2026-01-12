```yaml
number: 18705
title: "[ty] Offer \"Did you mean...?\" suggestions for unresolved `from` imports and unresolved attributes"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex-brent/did-you-mean
created_at: 2025-06-16T12:16:25Z
updated_at: 2025-06-17T10:10:36Z
url: https://github.com/astral-sh/ruff/pull/18705
synced_at: 2026-01-12T15:56:24Z
```

# [ty] Offer "Did you mean...?" suggestions for unresolved `from` imports and unresolved attributes

---

_@AlexWaygood_

## Summary

This PR (a collaboration with @ntBre) adds "Did you mean...?" suggestions for unresolved `from` imports and unresolved attributes. For example:

![image](https://github.com/user-attachments/assets/328a575b-f83b-46c2-ab11-4e8c9f6de8eb)

The implementation uses the [Levenshtein distance](https://en.wikipedia.org/wiki/Levenshtein_distance) algorithm to calculate which possible member of the type is a closest match to the unresolved member that triggered the diagnostic. This specific Levenshtein implementation is an almost exact 1:1 port of CPython's implementation [here](https://github.com/python/cpython/blob/b102f091feebe8597d14ac69b9fd982a4cda6c8d/Lib/traceback.py#L1578-L1743), and many of the tests in this PR are also ported from CPython's tests. You can see CPython's Levenshtein implementation in action if you access a nonexistent attribute or import in the REPL:

```pycon
Python 3.13.1 (main, Jan  3 2025, 12:04:03) [Clang 15.0.0 (clang-1500.3.9.4)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import collections
>>> collections.dequee
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    collections.dequee
AttributeError: module 'collections' has no attribute 'dequee'. Did you mean: 'deque'?
>>> from asyncio import gett_event_lop
Traceback (most recent call last):
  File "<python-input-2>", line 1, in <module>
    from asyncio import gett_event_lop
ImportError: cannot import name 'gett_event_lop' from 'asyncio' (/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/asyncio/__init__.py). Did you mean: 'get_event_loop'?
>>> 
```

Our motivation for copying CPython's implementation almost exactly is that CPython has had this feature for several Python versions now, and during that time many bug reports have been filed regarding incorrect suggestions, which have since been fixed. This implementation is thus very well "battle-tested" by this point; we can say with a reasonable degree of confidence that it gives good suggestions for unresolved members in the Python context.

## Test Plan

- Unit tests in `levenshtein.rs`
- Mdtest integration tests

---

Co-authored-by: Brent Westbrook <brentrwestbrook@gmail.com>

---

_Review requested from @carljm by @AlexWaygood on 2025-06-16 12:16_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-16 12:16_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-16 12:16_

---

_Label `ty` added by @AlexWaygood on 2025-06-16 12:16_

---

_Label `diagnostics` added by @AlexWaygood on 2025-06-16 12:16_

---

_@AlexWaygood reviewed on 2025-06-16 12:23_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/all_members.rs`:1 on 2025-06-16 12:23_

This module has been renamed from `ide_support` to `all_members` because it's no longer just used for autocompletions; it's now also used for diagnostic suggestions.

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-06-16 12:28_

---

_Comment by @AlexWaygood on 2025-06-16 12:30_

Mypy_primer is failing because this `.unwrap()` call is panicking when checking `sympy`:

https://github.com/astral-sh/ruff/blob/869d7bf9a8c037a51e5ac319ee2286987bc54d76/crates/ty_python_semantic/src/types/ide_support.rs#L189

Which seems like it is probably a pre-existing bug exposed by this PR (but we obviously won't be able to land this PR without fixing it!).

---

_Comment by @AlexWaygood on 2025-06-16 13:09_

Minimal repro for the primer panic on this branch:

```py
class Point:
    def orthogonal_direction(self):
        self[0].is_zero


def test_point(p2: Point):
    p2.y
```

---

_Comment by @AlexWaygood on 2025-06-16 13:15_

And yes, it's easy to trigger the panic on `main` by adding this test:

```diff
diff --git a/crates/ty_ide/src/completion.rs b/crates/ty_ide/src/completion.rs
index a8b93080b4..9f0a013b31 100644
--- a/crates/ty_ide/src/completion.rs
+++ b/crates/ty_ide/src/completion.rs
@@ -1835,6 +1835,23 @@ f"{f.<CURSOR>
         test.assert_completions_include("method");
     }
 
+    #[test]
+    fn no_panic_for_attribute_table_that_contains_subscript() {
+        let test = cursor_test(
+            r#"
+class Point:
+    def orthogonal_direction(self):
+        self[0].is_zero
+
+def test_point(p2: Point):
+    p2.<CURSOR>
+"#,
+        );
+
+        let completions = test.completions();
+        assert_eq!(completions, "orthogonal_direction");
+    }

```

---

_Comment by @AlexWaygood on 2025-06-16 13:37_

The primer panic should be fixed by https://github.com/astral-sh/ruff/pull/18707, but I won't attempt to rebase this PR branch on top of that as the commit history got somewhat messy here 😆

---

_Comment by @AlexWaygood on 2025-06-16 22:25_

Mypy_primer is now timing out, but it appears that it ran ty successfully on all selected projects. I'm guessing it's taking _ages_ to compute the diff between `old_ty`'s results and `new_ty`'s results, because there are many, many diagnostics where the message will change like so:

```diff
- error[unresolved-attribute] Type `Foo` has no attribute `bar`
+ error[unresolved-attribute] Type `Foo` has no attribute `bar`: Did you mean `baz`?
```

So this PR is ready for review!

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/diagnostic/levenshtein.rs`:70 on 2025-06-17 01:36_

Not sure I understand this. It sounds like this is saying that we consider an _access_ of `self.attribute` (no assignment) sufficient to include `attribute` in `all_members`? Do we really do that? Why?

---

_@carljm approved on 2025-06-17 01:44_

I reviewed the tests and did a high-level review of the implementation.

This will probably make us look a lot slower on early benchmarks of codebases where we raise a lot of AttributeError false positives, but so it goes; we should eliminate the false positive AttributeErrors.

Very cool feature!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic/levenshtein.rs`:70 on 2025-06-17 09:48_

> It sounds like this is saying that we consider an _access_ of `self.attribute` (no assignment) sufficient to include `attribute` in `all_members`? Do we really do that?

That does appear to be the case, yes. If I make this diff to the PR branch:

```diff
diff --git a/crates/ty_python_semantic/src/types/diagnostic/levenshtein.rs b/crates/ty_python_semantic/src/types/diagnostic/levenshtein.rs
index 11fc59674e..ff1fc86a28 100644
--- a/crates/ty_python_semantic/src/types/diagnostic/levenshtein.rs
+++ b/crates/ty_python_semantic/src/types/diagnostic/levenshtein.rs
@@ -66,18 +66,6 @@ where
         return None;
     }
 
-    // Filter out the unresolved member itself.
-    // Otherwise (due to our implementation of implicit instance attributes),
-    // we end up giving bogus suggestions like this:
-    //
-    // ```python
-    // class Foo:
-    //     _attribute = 42
-    //     def bar(self):
-    //         print(self.attribute)  # error: unresolved attribute `attribute`; did you mean `attribute`?
-    // ```
-    let options = options.filter(|name| name != unresolved_member);
-
```

then we get these changes to the snapshots:

![image](https://github.com/user-attachments/assets/f88e1e8b-2522-44bf-87db-a8e82a76baea)

And those seem to be self-evidently worse "Did you mean?" suggestions 😆

> Why?

I'm not sure. I agree that it seems a bit suspect. @sharkdp, do you have any ideas off the top of your head for why `attribute` might be included in the list returned by `all_members` here? If not, I can look into this as a followup.

---

_@AlexWaygood reviewed on 2025-06-17 09:48_

---

_Merged by @AlexWaygood on 2025-06-17 10:10_

---

_Closed by @AlexWaygood on 2025-06-17 10:10_

---

_Branch deleted on 2025-06-17 10:10_

---
