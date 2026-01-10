```yaml
number: 20807
title: "[ty] Add some completion ranking improvements"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/completion-improvements
created_at: 2025-10-10T18:32:44Z
updated_at: 2025-10-15T09:13:05Z
url: https://github.com/astral-sh/ruff/pull/20807
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Add some completion ranking improvements

---

_Pull request opened by @BurntSushi on 2025-10-10 18:32_

This PR contains a few improvements for completions:

* We now take the text that has been typed into consideration when
generating completions in all cases. Previously, we only did this for
auto-import completions.
* The keyword values `None`, `True` and `False` are now included in
scoped completions.
* Completions are not shown within comments.
* Completions are not shown within strings.
* Completions are _still_ shown within the interpolated segments of
f-strings and t-strings.

We add some new evaluation tasks to capture some of these cases. (And
lots of tests for the string detection logic.)

**Note:** I did make some changes to the `TokenFlags` representation.
I'm open to other ideas. But I wanted to flag this since `TokenFlags`
is used within the representation of a `Token`, which is a fundamental
data type inside of Ruff. Thankfully, even though `TokenFlags` gets
bigger, `Token` itself does not (because of padding/alignment).


---

_Review requested from @carljm by @BurntSushi on 2025-10-10 18:32_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-10-10 18:32_

---

_Review requested from @sharkdp by @BurntSushi on 2025-10-10 18:32_

---

_Review requested from @dcreager by @BurntSushi on 2025-10-10 18:32_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-10-10 18:32_

---

_Review requested from @dhruvmanila by @BurntSushi on 2025-10-10 18:32_

---

_Comment by @github-actions[bot] on 2025-10-10 18:34_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Review request for @dcreager removed by @BurntSushi on 2025-10-10 18:35_

---

_Review request for @carljm removed by @BurntSushi on 2025-10-10 18:35_

---

_Review request for @AlexWaygood removed by @BurntSushi on 2025-10-10 18:35_

---

_Comment by @github-actions[bot] on 2025-10-10 18:35_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `server` added by @AlexWaygood on 2025-10-10 18:41_

---

_Label `ty` added by @AlexWaygood on 2025-10-10 18:41_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:297 on 2025-10-10 18:51_

`True` is an instance of the `bool` class; it is not the `bool` class object itself. So IIUC what's going on here, this should be

```suggestion
            ty: Some(known.to_instance(db)),
```

(the same applies for the other two keywords)

But also, a more precise type for `True` than `bool` is `Literal[True]` (`Type::BooleanLiteral(true)` in our representation). None is just an instance of `NoneType`, though; there's no more precise type there.

---

_Comment by @github-actions[bot] on 2025-10-10 18:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@AlexWaygood reviewed on 2025-10-10 19:00_

---

_@BurntSushi reviewed on 2025-10-10 22:06_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:297 on 2025-10-10 22:06_

Derp. Fixed!

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:283 on 2025-10-11 18:00_

(sorry for the back-and-forth on this) -- I now remember that we actually have a helper function for getting "instance of `NoneType`" specifically, since that's quite common in ty:

```suggestion
        ("None", Type::none(db)),
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/token.rs`:742 on 2025-10-11 18:43_

This has a _bit_ of a bad smell for me, because for all the `*Flags` structs in the `ruff_python_ast` crate, we simply use the absence of the `DOUBLE_QUOTES` flag to determine if a string uses single quotes. This change reduces the symmetry between this bitflag and all those bitflags:

https://github.com/astral-sh/ruff/blob/dc64c086336148c57db14060c86f448351710b2e/crates/ruff_python_ast/src/nodes.rs#L809-L833

https://github.com/astral-sh/ruff/blob/dc64c086336148c57db14060c86f448351710b2e/crates/ruff_python_ast/src/nodes.rs#L1398-L1431

https://github.com/astral-sh/ruff/blob/dc64c086336148c57db14060c86f448351710b2e/crates/ruff_python_ast/src/nodes.rs#L1826-L1850

https://github.com/astral-sh/ruff/blob/dc64c086336148c57db14060c86f448351710b2e/crates/ruff_python_ast/src/nodes.rs#L2021-L2075

It seems what you're looking for is a way to determine whether a token with `TokenKind::Unknown` represents an unclosed string. So maybe we could just have an `IS_STRING` (or `ANY_STRING`?) flag that is always set when the lexer starts lexing a string? Something like this?

```diff
--- a/crates/ruff_python_parser/src/lexer.rs
+++ b/crates/ruff_python_parser/src/lexer.rs
@@ -760,10 +760,10 @@ impl<'src> Lexer<'src> {
         #[cfg(debug_assertions)]
         debug_assert_eq!(self.cursor.previous(), quote);
 
+        self.current_flags |= TokenFlags::IS_STRING;
+
         if quote == '"' {
             self.current_flags |= TokenFlags::DOUBLE_QUOTES;
-        } else if quote == '\'' {
-            self.current_flags |= TokenFlags::SINGLE_QUOTES;
         }
 
         if self.cursor.eat_char2(quote, quote) {
@@ -924,10 +924,10 @@ impl<'src> Lexer<'src> {
         #[cfg(debug_assertions)]
         debug_assert_eq!(self.cursor.previous(), quote);
 
+        self.current_flags |= TokenFlags::IS_STRING;
+
         if quote == '"' {
             self.current_flags |= TokenFlags::DOUBLE_QUOTES;
-        } else if quote == '\'' {
-            self.current_flags |= TokenFlags::SINGLE_QUOTES;
         }
 
         // If the next two characters are also the quote character, then we have a triple-quoted
diff --git a/crates/ruff_python_parser/src/token.rs b/crates/ruff_python_parser/src/token.rs
index c26254f23f..329c65812c 100644
--- a/crates/ruff_python_parser/src/token.rs
+++ b/crates/ruff_python_parser/src/token.rs
@@ -736,10 +736,12 @@ impl fmt::Display for TokenKind {
 bitflags! {
     #[derive(Clone, Copy, Debug, PartialEq, Eq)]
     pub struct TokenFlags: u16 {
+        /// The token is probably a string.
+        /// Note that this will not only be set for string tokens, but also for
+        /// tokens with [`TokenKind::Unknown`] that the lexer suspects to be strings.
+        const IS_STRING = 1 << 0;
         /// The token is a string with double quotes (`"`).
-        const DOUBLE_QUOTES = 1 << 0;
-        /// The token is a string with single quotes (`'`).
-        const SINGLE_QUOTES = 1 << 1;
+        const DOUBLE_QUOTES = 1 << 1;
         /// The token is a triple-quoted string i.e., it starts and ends with three consecutive
         /// quote characters (`"""` or `'''`).
         const TRIPLE_QUOTED_STRING = 1 << 2;
@@ -844,12 +846,12 @@ impl TokenFlags {
         self.intersects(TokenFlags::RAW_STRING)
     }
 
-    /// Returns `true` when the flags contain an explicit double or single quotes.
+    /// Returns `true` if this token is probably a string.
     ///
-    /// This is useful for determining whether an "unknown" token corresponds
-    /// to the start of a string.
-    pub const fn has_quotes(self) -> bool {
-        self.intersects(TokenFlags::DOUBLE_QUOTES) || self.intersects(TokenFlags::SINGLE_QUOTES)
+    /// Note that this may be set for tokens with [`TokenKind::Unknown`]
+    /// as well as for tokens with [`TokenKind::String`].
+    pub const fn is_string(self) -> bool {
+        self.intersects(TokenFlags::IS_STRING)
     }
 }
 
diff --git a/crates/ty_ide/src/completion.rs b/crates/ty_ide/src/completion.rs
index 2322389222..2a8be276b4 100644
--- a/crates/ty_ide/src/completion.rs
+++ b/crates/ty_ide/src/completion.rs
@@ -863,10 +863,10 @@ fn is_in_unclosed_string(parsed: &ParsedModuleRef, offset: TextSize) -> bool {
     for (i, t) in tokens.iter().enumerate().rev() {
         match t.kind() {
             TokenKind::Unknown => {
-                // It's possible to lex an unknown token with single/double
-                // quotes. If we got here, then we assume it is the opening
-                // of an unclosed string.
-                if t.flags().has_quotes() {
+                // If the file is currently invalid syntax, the token kind may be
+                // `TokenKind::Unknown`, so also check the flags to see if the
+                // lexer suspects that it's dealing with a string.
+                if t.flags().is_string() {
                     return true;
                 }
                 // Otherwise, we permit any kind of unknown token to be
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:815 on 2025-10-11 18:54_

```suggestion
    if last.end() < offset {
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:818 on 2025-10-11 18:55_

```suggestion
    Some(source[last.range()].to_string())
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:2876 on 2025-10-11 18:58_

you can delete the TODO comment here -- it's now done :-)

---

_@AlexWaygood approved on 2025-10-11 19:11_

Nice!

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/token.rs`:742 on 2025-10-12 06:14_

Hmm, I think my preferred solution would just be to always lex strings as strings, even if they're unclosed. But I'd have to play with it to see if this breaks any assumptions in Ruff

---

_@MichaReiser reviewed on 2025-10-12 06:14_

---

_@MichaReiser reviewed on 2025-10-12 14:54_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/token.rs`:742 on 2025-10-12 14:54_

I'll give this a try Tomorrow. 

---

_Review request for @sharkdp removed by @sharkdp on 2025-10-13 08:04_

---

_Comment by @sharkdp on 2025-10-13 08:05_

I planned to take a look at this, but it looks like we have two reviews already.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/token.rs`:742 on 2025-10-14 09:32_

See https://github.com/astral-sh/ruff/pull/20848

I'll rebase this PR once https://github.com/astral-sh/ruff/pull/20848 lands

---

_@MichaReiser reviewed on 2025-10-14 09:32_

---

_Comment by @MichaReiser on 2025-10-15 08:31_

Hmm, I think the evaluation results differ between mac and linux?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:70 on 2025-10-15 08:47_

```suggestion
pub(crate) use class::{ClassLiteral, ClassType, GenericAlias, KnownClass};
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4559 on 2025-10-15 08:48_

```suggestion
    pub(crate) fn to_instance(self, db: &dyn Db) -> Type<'_> {
```

---

_@AlexWaygood reviewed on 2025-10-15 08:48_

---

_Comment by @MichaReiser on 2025-10-15 08:56_

Sorry @BurntSushi, I'll squash-merge the commits because @AlexWaygood and I don't have as good a commit hygiene as you.

---

_Merged by @MichaReiser on 2025-10-15 08:59_

---

_Closed by @MichaReiser on 2025-10-15 08:59_

---

_Branch deleted on 2025-10-15 08:59_

---
