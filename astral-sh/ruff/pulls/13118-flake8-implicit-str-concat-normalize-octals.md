```yaml
number: 13118
title: "[`flake8-implicit-str-concat`] Normalize octals before merging concatenated strings in `single-line-implicit-string-concatenation` (`ISC001`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: octal-escape
created_at: 2024-08-26T22:16:47Z
updated_at: 2024-08-27T18:02:32Z
url: https://github.com/astral-sh/ruff/pull/13118
synced_at: 2026-01-12T15:55:43Z
```

# [`flake8-implicit-str-concat`] Normalize octals before merging concatenated strings in `single-line-implicit-string-concatenation` (`ISC001`)

---

_@dylwil3_

This PR pads the last octal (if there is one) to three digits before concatenating strings in the fix for `ISC001`.

For example: `"\12""0"` is fixed to `"\0120"`.

Closes #12936.


---

_Comment by @github-actions[bot] on 2024-08-26 22:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-08-27 14:46_

This is very clever. Good job! Two small points:

1. The `.to_string()` call on line 175 will clone the underlying data, which could be quite costly -- and is unnecessary in the happy path, since most strings don't have octal escapes in them. We can avoid this by using a [`Cow`](https://doc.rust-lang.org/std/borrow/enum.Cow.html) -- something like this? (The diff is relative to your branch)

```diff
--- a/crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/implicit.rs
+++ b/crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/implicit.rs
@@ -1,3 +1,5 @@
+use std::borrow::Cow;
+
 use itertools::Itertools;
 
 use ruff_diagnostics::{Diagnostic, Edit, Fix, FixAvailability, Violation};
@@ -173,11 +175,11 @@ fn concatenate_strings(a_range: TextRange, b_range: TextRange, locator: &Locator
     }
 
     let mut a_body =
-        a_text[a_leading_quote.len()..a_text.len() - a_trailing_quote.len()].to_string();
+        Cow::Borrowed(&a_text[a_leading_quote.len()..a_text.len() - a_trailing_quote.len()]);
     let b_body = &b_text[b_leading_quote.len()..b_text.len() - b_trailing_quote.len()];
 
     if a_leading_quote.find(['r', 'R']).is_none() {
-        a_body = normalize_ending_octal(&a_body);
+        normalize_ending_octal(&mut a_body);
     }
 
     let concatenation = format!("{a_leading_quote}{a_body}{b_body}{a_trailing_quote}");
@@ -191,10 +193,10 @@ fn concatenate_strings(a_range: TextRange, b_range: TextRange, locator: &Locator
 
 /// Pads an octal at the end of the string
 /// to three digits, if necessary.
-fn normalize_ending_octal(text: &str) -> String {
+fn normalize_ending_octal(text: &mut Cow<'_, str>) {
     // Early return for short strings
     if text.len() < 2 {
-        return text.to_string();
+        return;
     }
 
     let mut rev_bytes = text.bytes().rev();
@@ -202,20 +204,19 @@ fn normalize_ending_octal(text: &str) -> String {
         // "\y" -> "\00y"
         if has_odd_consecutive_backslashes(&rev_bytes) {
             let prefix = &text[..text.len() - 2];
-            return format!("{prefix}\\00{}", last_byte as char);
+            *text = Cow::Owned(format!("{prefix}\\00{}", last_byte as char));
         }
         // "\xy" -> "\0xy"
-        if let Some(penultimate_byte @ b'0'..=b'7') = rev_bytes.next() {
+        else if let Some(penultimate_byte @ b'0'..=b'7') = rev_bytes.next() {
             if has_odd_consecutive_backslashes(&rev_bytes) {
                 let prefix = &text[..text.len() - 3];
-                return format!(
+                *text = Cow::Owned(format!(
                     "{prefix}\\0{}{}",
                     penultimate_byte as char, last_byte as char
-                );
+                ));
             }
         }
     }
-    text.to_string()
 }
```

2. I wonder if it's necessary to normalize the ending octal in the first string if the second string doesn't start with a digit. E.g. if I understand correctly, something like `"\12" "foo"` will be fixed as `"\012foo"` according to the logic in your PR -- but I _think_ it's safe to not apply the normalization logic in this case, and instead fix it as `\12foo`? What do you think?

---

_@AlexWaygood reviewed on 2024-08-27 14:59_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/implicit.rs`:228 on 2024-08-27 14:59_

nit: I'd probably rewrite the function like this, so that it receives an owned value rather than receiving a borrowed value that it immediately clones:

```suggestion
fn has_odd_consecutive_backslashes(mut itr: impl Iterator<Item = u8>) -> bool {
    let mut odd_backslashes: bool = false;
    while let Some(b'\\') = itr.next() {
        odd_backslashes = !odd_backslashes;
    }
    odd_backslashes
}
```

And then explicitly call `rev_bytes.clone()` at each callsite before passing the iterator into this function. That makes the signature of this function slightly less complicated, and it also makes the code more explicit about each clone that we're doing. (The clone shouldn't be _that_ expensive here for an iterator instance, but it's still good to be explicit about it!)

---

_@dylwil3 reviewed on 2024-08-27 17:04_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/implicit.rs`:228 on 2024-08-27 17:04_

Good suggestion! implemented

---

_Comment by @dylwil3 on 2024-08-27 17:17_

Re putting the `Cow`s to pasture:

The borrow checker complained when I tried to implement the fix you suggested (assuming I did it correctly), because I borrow `text` here:

```rust
let mut rev_bytes = text.bytes().rev();
```

and then try to modify it with `*text = Cow::Owned(...)`. I can try to mess around, but it's not immediately obvious how to avoid a clone/copy/some other memory allocation.

More fundamentally, I'm hoping you could help with a Rust confusion here. I would've thought that the `.to_string` wouldn't be much added cost because we eventually convert everything to a `String` (even on the happy path) here:

```rust
let concatenation = format!("{a_leading_quote}{a_body}{b_body}{a_trailing_quote}");
```

Maybe a more minimal version of my question is: Do you gain much in terms or memory/performance by doing

```rust
let a = &"xyz";
let s = format!("{a}")
// then a drops out of scope
```
vs
```rust
let a = "xyz".to_string();
let s = format!("{a}")
// then a drops out of scope
```
?

Or does the compiler end up making those roughly the same?

(Obviously the first code is better in this contrived example, but the question remains.)

Sorry for the long-ish question, and thanks for the very helpful review!

---

_Comment by @AlexWaygood on 2024-08-27 17:48_

> Or does the compiler end up making those roughly the same?

Ummmm... I'm not entirely sure exactly what optimisations are permitted here! You may well be right that the compiler is smart enough to "see through" the allocation and optimise it away -- but I'm not sure if that's a permitted optimisation, or if it's one the compiler's smart enough to make. I _could_ go and try to investigate exactly whether it does make this optimisation or not -- but I'm not sure the exact optimisations the compiler is likely to make here are things we should be relying on anyway ;) So I think it's better to use a `Cow` here!

---

_Comment by @AlexWaygood on 2024-08-27 17:50_

> The borrow checker complained when I tried to implement the fix you suggested (assuming I did it correctly), because I borrow `text` here:

I pushed the change I was suggesting to your PR branch in https://github.com/astral-sh/ruff/pull/13118/commits/25805a2992889918b3b5626c5e60139ca756037d :-) the key to avoiding the borrow-checker complaints is to have `normalize_ending_octal()` mutate the existing value _in-place_ rather than returning a new value

---

_@AlexWaygood approved on 2024-08-27 17:50_

Thanks again!

---

_Comment by @dylwil3 on 2024-08-27 17:51_

Makes sense, and thank you that was very helpful!

---

_Renamed from "[flake8-implicit-str-concat] Normalize octals before concatenating strings in `single-line-implicit-string-concatenation (ISC001)`" to "[`flake8-implicit-str-concat`] Normalize octals before merging concatenated strings in `single-line-implicit-string-concatenation` (`ISC001`)" by @AlexWaygood on 2024-08-27 17:53_

---

_Label `bug` added by @AlexWaygood on 2024-08-27 17:53_

---

_Label `fixes` added by @AlexWaygood on 2024-08-27 17:53_

---

_Merged by @AlexWaygood on 2024-08-27 17:53_

---

_Closed by @AlexWaygood on 2024-08-27 17:53_

---

_Branch deleted on 2024-08-27 17:53_

---
