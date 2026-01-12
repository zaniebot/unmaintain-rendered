```yaml
number: 21176
title: "Avoid extra parentheses for long `match` patterns with `as` captures"
type: pull_request
state: merged
author: ntBre
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: brent/format-match-as-captures
created_at: 2025-10-31T20:54:58Z
updated_at: 2025-11-03T22:06:54Z
url: https://github.com/astral-sh/ruff/pull/21176
synced_at: 2026-01-12T15:57:18Z
```

# Avoid extra parentheses for long `match` patterns with `as` captures

---

_@ntBre_

Summary
--

This PR fixes #17796 by taking the approach mentioned in https://github.com/astral-sh/ruff/issues/17796#issuecomment-2847943862 of simply recursing into the `MatchAs` patterns when checking if we need parentheses. This allows us to reuse the parentheses in the inner pattern before also breaking the `MatchAs` pattern itself:

```diff
 match class_pattern:
     case Class(xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx) as capture:
         pass
-    case (
-        Class(xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx) as capture
-    ):
+    case Class(
+        xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
+    ) as capture:
         pass
-    case (
-        Class(
-            xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
-        ) as capture
-    ):
+    case Class(
+        xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
+    ) as capture:
         pass
     case (
         Class(
@@ -685,13 +683,11 @@
 match sequence_pattern_brackets:
     case [xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx] as capture:
         pass
-    case (
-        [xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx] as capture
-    ):
+    case [
+        xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
+    ] as capture:
         pass
-    case (
-        [
-            xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
-        ] as capture
-    ):
+    case [
+        xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
+    ] as capture:
         pass
```

I haven't really resolved the question of whether or not it's okay always to recurse, but I'm hoping the ecosystem check on this PR might shed some light on that.

Test Plan
--

New tests based on the issue and then reviewing the ecosystem check here


---

_Comment by @github-actions[bot] on 2025-10-31 21:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `formatter` added by @ntBre on 2025-10-31 22:12_

---

_Label `preview` added by @ntBre on 2025-10-31 22:12_

---

_Comment by @MichaReiser on 2025-11-01 00:39_

> I haven't really resolved the question of whether or not it's okay always to recurse, but I'm hoping the ecosystem check on this PR might shed some light on that.

😆 

---

_Comment by @dylwil3 on 2025-11-01 12:13_

To save you future pain, or rather to bring future pain into the present, I recommend adding some fixtures with absurd comment placement 😄

---

_Comment by @ntBre on 2025-11-03 19:37_

> To save you future pain, or rather to bring future pain into the present, I recommend adding some fixtures with absurd comment placement 😄

Thanks for the suggestion! I added some more test cases, but I'm definitely open to additional suggestions. I'm not sure I captured the full absurdity available here.

I guess I'll tentatively call it okay always to recurse since nothing showed up in the ecosystem check. From what I remember with the syntax errors, many projects still support 3.9 and don't use much pattern matching in general, so I guess this isn't too surprising. The main case I though to worry about was this test case:

```py
match class_pattern:
    case Class(
        xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    ) as really_really_really_really_really_really_really_really_really_really_really_really_long_capture:
        pass
```

but it both wraps the `Class` and the whole `MatchAs` pattern, as I hoped.

Open to suggestions there as well!

---

_Marked ready for review by @ntBre on 2025-11-03 19:37_

---

_Review requested from @MichaReiser by @ntBre on 2025-11-03 19:37_

---

_@MichaReiser approved on 2025-11-03 21:50_

---

_Merged by @ntBre on 2025-11-03 22:06_

---

_Closed by @ntBre on 2025-11-03 22:06_

---

_Branch deleted on 2025-11-03 22:06_

---
