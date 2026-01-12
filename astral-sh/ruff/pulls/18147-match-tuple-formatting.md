```yaml
number: 18147
title: Match tuple formatting
type: pull_request
state: merged
author: maxmynter
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: match-tuple-formatting
created_at: 2025-05-17T04:51:15Z
updated_at: 2025-05-22T05:52:21Z
url: https://github.com/astral-sh/ruff/pull/18147
synced_at: 2026-01-12T15:56:13Z
```

# Match tuple formatting

---

_@maxmynter_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

Closes #17969 

## Summary
Add check to sequence type determination of `match` formatting to distinguish between a list and a tuple whose leading element is a list. 

Previously, we only checked for an opening bracket, `[`, now we additionally check for top level commata to determine if it is a tuple and return the according type.

The resulting formatting behaviour is consistent with Black.
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Added a test case to ruff's tests. 
Note: this case is not covered in tests imported from Black. 

Manually compared Black, and this fix on the example from the issue. 
<!-- How was it tested? -->


---

_Review requested from @MichaReiser by @maxmynter on 2025-05-17 04:51_

---

_Comment by @github-actions[bot] on 2025-05-17 04:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `formatter` added by @MichaReiser on 2025-05-17 15:17_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/pattern/pattern_match_sequence.rs`:97 on 2025-05-17 15:22_

I think the proper fix here is to only look at the text of the outer pattern by using:

```rust
let text = &source[TextRange::new(pattern.start(), pattern.values.first().map(Ranged::end).unwrap_or(pattern.end())];
```

This will return `` `case [], []:` because the `[` belongs to the inner pattern (and not the outer.

We can then use the same text for the elif on line 97



---

_@MichaReiser reviewed on 2025-05-17 15:23_

Thank you. I've a small suggestion which may make this less fragile.

---

_@maxmynter reviewed on 2025-05-18 02:06_

---

_Review comment by @maxmynter on `crates/ruff_python_formatter/src/pattern/pattern_match_sequence.rs`:97 on 2025-05-18 02:06_

I'm not sure if my understanding is correct.

On the on the first example of the issue it gives
```bash
[crates/ruff_python_formatter/src/pattern/pattern_match_sequence.rs:82:29] &source[TextRange::new(pattern.start(),
pattern.patterns.first().map(Ranged::end).unwrap_or(pattern.end()),)] = "[]"
```


Do you mean something  like this: 
```rust
    pub(crate) fn from_pattern(pattern: &PatternMatchSequence, source: &str) -> SequenceType {
        let first_element = dbg!(&source[TextRange::new(
            pattern.start(),
            pattern
                .patterns
                .first()
                .map(Ranged::end)
                .unwrap_or(pattern.end()),
        )]);
        if first_element.starts_with("[") {
            if first_element == &source[pattern.range()] {
                SequenceType::List
            } else {
                SequenceType::TupleNoParens
            }
        } else if first_element.starts_with('(') {
```
(this breaks format and black compatibility tests, though). 

We cannot check for a leading `[` because that way we cannot discriminate between a tuple that starts with a list, `[],_` and a list, `[_]`.

---

_@MichaReiser reviewed on 2025-05-18 15:27_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/pattern/pattern_match_sequence.rs`:97 on 2025-05-18 15:27_

Sorry, I messed up the code example. We should take the `start` of the first pattern, not the end

```rust
let text = &source[TextRange::new(pattern.start(), pattern.values.first().map(Ranged::start).unwrap_or(pattern.end())];
```

---

_@maxmynter reviewed on 2025-05-19 01:14_

---

_Review comment by @maxmynter on `crates/ruff_python_formatter/src/pattern/pattern_match_sequence.rs`:97 on 2025-05-19 01:14_

This implementation
```rust
    pub(crate) fn from_pattern(pattern: &PatternMatchSequence, source: &str) -> SequenceType {
        let text = &source[TextRange::new(
            pattern.start(),
            pattern
                .patterns // Note: I use `.patterns` as `.values` doesn't exist.
                .first()
                .map(Ranged::start)
                .unwrap_or(pattern.end()),
        )];
        if text.starts_with('[') {
            SequenceType::List
        } else if text.starts_with('(') {
...
```
fails for nested lists. E.g. the following Black consistency test: 
```bash
   65       │-    case [
         57 │+    case (
   66    58 │         [[5], (6)],
   67    59 │         [7],
   68       │-    ]:
         60 │+    ):
   69    61 │         pass
   70    62 │     case _:
   ```
   
   The rationale behind counting top level commata was they are the defining [characteristic](https://docs.python.org/3/tutorial/datastructures.html#tuples-and-sequences) of a tuple.
   
   Or am I still misunderstanding? 
   

---

_@MichaReiser reviewed on 2025-05-19 06:00_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/pattern/pattern_match_sequence.rs`:97 on 2025-05-19 06:00_

This change is good! It reduces another incompatibility with black. But I do think that we also need to account for a trailing comma like this

```patch
Index: crates/ruff_python_formatter/src/pattern/pattern_match_sequence.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_python_formatter/src/pattern/pattern_match_sequence.rs b/crates/ruff_python_formatter/src/pattern/pattern_match_sequence.rs
--- a/crates/ruff_python_formatter/src/pattern/pattern_match_sequence.rs	(revision 660375d429c41878c9a8866c383d5f7ec060c229)
+++ b/crates/ruff_python_formatter/src/pattern/pattern_match_sequence.rs	(date 1747634249453)
@@ -79,9 +79,27 @@
 
 impl SequenceType {
     pub(crate) fn from_pattern(pattern: &PatternMatchSequence, source: &str) -> SequenceType {
-        if source[pattern.range()].starts_with('[') {
+        let before_first_pattern = &source[TextRange::new(
+            pattern.start(),
+            pattern
+                .patterns
+                .first()
+                .map(Ranged::start)
+                .unwrap_or(pattern.end()),
+        )];
+
+        let after_last_pattern = &source[TextRange::new(
+            pattern
+                .patterns
+                .last()
+                .map(Ranged::end)
+                .unwrap_or(pattern.start()),
+            pattern.end(),
+        )];
+
+        if before_first_pattern.starts_with('[') && !after_last_pattern.ends_with(',') {
             SequenceType::List
-        } else if source[pattern.range()].starts_with('(') {
+        } else if before_first_pattern.starts_with('(') {
             // If the pattern is empty, it must be a parenthesized tuple with no members. (This
             // branch exists to differentiate between a tuple with and without its own parentheses,
             // but a tuple without its own parentheses must have at least one member.)
```

To correctly hanlde 

```py
match more := (than, one), indeed,:
    case [[5], (6)],:
        pass
    case _:
        pass
```

but we should add a test for this. We should also add tests that this change doesn't the formatting of any already formatted code (where Ruff added the extra `[` `]` because we otherwise need to gate this change behind preview mode.



---

_@maxmynter reviewed on 2025-05-21 15:29_

---

_Review comment by @maxmynter on `crates/ruff_python_formatter/src/pattern/pattern_match_sequence.rs`:97 on 2025-05-21 15:29_

Allright, i've adapted the code according to your suggestions in [fdcebbc](https://github.com/astral-sh/ruff/pull/18147/commits/fdcebbc6bb8b8825a048737417491e9e5a29d0c0)

The updates to the Black compatibility are in [0ac564f](https://github.com/astral-sh/ruff/pull/18147/commits/0ac564fe607afe47a72e7e0e31c5e24986326386) -- thanks for pointing out that the changes are desired. I was very much in a red things = bad mindset. Took some time to learn about how you do this after. :) 

The previously applied formatting is not changed back. So i don't think preview gating is necessary. I added a test for it in [019e5c0](https://github.com/astral-sh/ruff/pull/18147/commits/019e5c01c9ba738f0c33b6ec9c6cff14e8db1fbd).

---

_Review requested from @MichaReiser by @maxmynter on 2025-05-21 15:30_

---

_Comment by @MichaReiser on 2025-05-22 05:51_

Nice, thank you

---

_Label `bug` added by @MichaReiser on 2025-05-22 05:51_

---

_Merged by @MichaReiser on 2025-05-22 05:52_

---

_Closed by @MichaReiser on 2025-05-22 05:52_

---
