```yaml
number: 11564
title: Implement a common trait for the string flags
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: stringflags-trait2
created_at: 2024-05-27T10:30:20Z
updated_at: 2024-05-28T04:16:07Z
url: https://github.com/astral-sh/ruff/pull/11564
synced_at: 2026-01-12T15:55:38Z
```

# Implement a common trait for the string flags

---

_@AlexWaygood_

## Summary

This PR implements a common trait for the four "string flags" types in `crates/ruff_python_ast/src/nodes.rs`:
- `FStringFlags`
- `StringLiteralFlags`
- `ByteStringFlags`
- `AnyStringFlags`

This has the following advantages:
- It means that we enforce a consistent interface between the four types. Currently this is implicit; this PR makes it explicit, and ensures that they don't accidentally become inconsistent in the future.
- Various methods (`quote_str()`, `quote_len()`, `format_string_contents()`, etc.) that were previously only available on `AnyStringFlags` are now available on all four structs. This means that there is no need to convert the more precise types into the more general type, which was previously required if you wanted to access these methods. The PR updates two pylint rules so that they no longer make this conversion from more precise types to `AnyStringFlags`; instead, the trait is brought into scope so that the rules can access the methods directly without making any conversion.

It has the following disadvantages that I am aware of:
- The trait now needs to be explicitly brought into scope if you want to use any of the methods that come for free by virtue of implementing the trait. This might make these methods less discoverable for people implementing new linter rules
- It's more code for us to maintain, and there are currently relatively few uses of the methods that the trait provides default implementations for

This idea was suggested by @dhruvmanila in https://github.com/astral-sh/ruff/pull/11406#discussion_r1598494602

## Test Plan

`cargo test`. There should also be 0 ecosystem hits, as this is an internal refactor.


---

_Label `internal` added by @AlexWaygood on 2024-05-27 10:30_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-05-27 10:30_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2024-05-27 10:30_

---

_Comment by @github-actions[bot] on 2024-05-27 10:51_

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

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1356 on 2024-05-27 12:28_

I would just call this `StringFlags`. All traits are abstract, so naming the type `Abstract` adds little value. 



---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1372 on 2024-05-27 12:29_

I would probably remove the mutation methods from the trait and limit it to query methods only, mainly because I think it's unlikely that someone wants to mutate any string flags.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1523 on 2024-05-27 12:36_

I would suggest to remove the non trait method from `FStringFlags` and move the implementation into the trait implementation (same for other methods). It avoids confusion where it's unclear which of the two methods with the exact same signature to use. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_quotes/helpers.rs`:6 on 2024-05-27 12:38_

I kind of prefer taking `AnyStringFlags` here because it avoids monomorphisation. 

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1366 on 2024-05-27 12:40_

I'm inclined to return `AnyStringPrefix` here to avoid an impl return type.

---

_@MichaReiser approved on 2024-05-27 12:40_

I'm somewhat conflicted on this change. 

I do like the API alignment that it brings but it introduces a lot of code and `AbstractStringFlags` and `AnyStringFlags` kind of have the same purpose: Allow representing any string flags. That's where I prefer `AnyStringFlags` because we don't need the "openness" of traits, we know all implementations. 



---

_@AlexWaygood reviewed on 2024-05-27 13:34_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1366 on 2024-05-27 13:34_

Yeah, makes sense. `AnyStringPrefix` has all of the same methods, and is strictly more precise, since you can narrow it later to figure out which variant you have.

---

_@AlexWaygood reviewed on 2024-05-27 13:59_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1523 on 2024-05-27 13:59_

The main reason I don't do this right now is it increases the diff quite a bit: the trait has to be brought into scope in a lot more places. To me it feels unintuitive that I'd have to bring a trait into scope in order to access these methods... but maybe that's because I'm still relatively new to Rust 🙃

This is the diff if the implementation is moved into the trait implementation:

<details>

```diff
diff --git a/crates/ruff_linter/src/directives.rs b/crates/ruff_linter/src/directives.rs
index 6225c2acb..b8568b4d9 100644
--- a/crates/ruff_linter/src/directives.rs
+++ b/crates/ruff_linter/src/directives.rs
@@ -4,6 +4,7 @@ use std::iter::Peekable;
 use std::str::FromStr;
 
 use bitflags::bitflags;
+use ruff_python_ast::StringFlags;
 use ruff_python_parser::lexer::LexResult;
 use ruff_python_parser::Tok;
 use ruff_text_size::{Ranged, TextLen, TextRange, TextSize};
diff --git a/crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs b/crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs
index 6d864ef52..a22ef2e04 100644
--- a/crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs
+++ b/crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs
@@ -1,7 +1,7 @@
 use ruff_diagnostics::{AlwaysFixableViolation, Diagnostic, Edit, Fix};
 use ruff_macros::{derive_message_formats, violation};
 use ruff_python_ast::visitor::{walk_f_string, Visitor};
-use ruff_python_ast::{self as ast, AnyStringFlags, StringLike};
+use ruff_python_ast::{self as ast, AnyStringFlags, StringFlags, StringLike};
 use ruff_source_file::Locator;
 use ruff_text_size::{Ranged, TextRange, TextSize};
 
diff --git a/crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs b/crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs
index fa38ffb73..e0ba88eef 100644
--- a/crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs
+++ b/crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs
@@ -2,7 +2,7 @@ use memchr::memchr_iter;
 
 use ruff_diagnostics::{AlwaysFixableViolation, Diagnostic, Edit, Fix};
 use ruff_macros::{derive_message_formats, violation};
-use ruff_python_ast::{AnyStringFlags, FStringElement, StringLike, StringLikePart};
+use ruff_python_ast::{AnyStringFlags, FStringElement, StringFlags, StringLike, StringLikePart};
 use ruff_source_file::Locator;
 use ruff_text_size::{Ranged, TextLen, TextRange, TextSize};
 
diff --git a/crates/ruff_python_ast/src/nodes.rs b/crates/ruff_python_ast/src/nodes.rs
index c8dde077c..ebfaf7b8d 100644
--- a/crates/ruff_python_ast/src/nodes.rs
+++ b/crates/ruff_python_ast/src/nodes.rs
@@ -1480,11 +1480,13 @@ impl FStringFlags {
             FStringPrefix::Regular
         }
     }
+}
 
+impl StringFlags for FStringFlags {
     /// Return `true` if the f-string is triple-quoted, i.e.,
     /// it begins and ends with three consecutive quote characters.
     /// For example: `f"""{bar}"""`
-    pub const fn is_triple_quoted(self) -> bool {
+    fn is_triple_quoted(self) -> bool {
         self.0.contains(FStringFlagsInner::TRIPLE_QUOTED)
     }
 
@@ -1492,7 +1494,7 @@ impl FStringFlags {
     /// used by the f-string's opener and closer:
     /// - `f"{"a"}"` -> `QuoteStyle::Double`
     /// - `f'{"a"}'` -> `QuoteStyle::Single`
-    pub const fn quote_style(self) -> Quote {
+    fn quote_style(self) -> Quote {
         if self.0.contains(FStringFlagsInner::DOUBLE) {
             Quote::Double
         } else {
@@ -1500,24 +1502,10 @@ impl FStringFlags {
         }
     }
 
-    pub const fn is_raw_string(self) -> bool {
+    fn is_raw_string(self) -> bool {
         self.0
             .intersects(FStringFlagsInner::R_PREFIX_LOWER.union(FStringFlagsInner::R_PREFIX_UPPER))
     }
-}
-
-impl StringFlags for FStringFlags {
-    fn quote_style(self) -> Quote {
-        self.quote_style()
-    }
-
-    fn is_triple_quoted(self) -> bool {
-        self.is_triple_quoted()
-    }
-
-    fn is_raw_string(self) -> bool {
-        self.is_raw_string()
-    }
 
     fn prefix(self) -> AnyStringPrefix {
         AnyStringPrefix::Format(self.prefix())
@@ -1913,12 +1901,14 @@ impl StringLiteralFlags {
             StringLiteralPrefix::Empty
         }
     }
+}
 
+impl StringFlags for StringLiteralFlags {
     /// Return the quoting style (single or double quotes)
     /// used by the string's opener and closer:
     /// - `"a"` -> `QuoteStyle::Double`
     /// - `'a'` -> `QuoteStyle::Single`
-    pub const fn quote_style(self) -> Quote {
+    fn quote_style(self) -> Quote {
         if self.0.contains(StringLiteralFlagsInner::DOUBLE) {
             Quote::Double
         } else {
@@ -1929,29 +1919,15 @@ impl StringLiteralFlags {
     /// Return `true` if the string is triple-quoted, i.e.,
     /// it begins and ends with three consecutive quote characters.
     /// For example: `"""bar"""`
-    pub const fn is_triple_quoted(self) -> bool {
+    fn is_triple_quoted(self) -> bool {
         self.0.contains(StringLiteralFlagsInner::TRIPLE_QUOTED)
     }
 
-    pub const fn is_raw_string(self) -> bool {
+    fn is_raw_string(self) -> bool {
         self.0.intersects(
             StringLiteralFlagsInner::R_PREFIX_LOWER.union(StringLiteralFlagsInner::R_PREFIX_UPPER),
         )
     }
-}
-
-impl StringFlags for StringLiteralFlags {
-    fn quote_style(self) -> Quote {
-        self.quote_style()
-    }
-
-    fn is_triple_quoted(self) -> bool {
-        self.is_triple_quoted()
-    }
-
-    fn is_raw_string(self) -> bool {
-        self.is_raw_string()
-    }
 
     fn prefix(self) -> AnyStringPrefix {
         AnyStringPrefix::Regular(self.prefix())
@@ -2278,11 +2254,13 @@ impl BytesLiteralFlags {
             ByteStringPrefix::Regular
         }
     }
+}
 
+impl StringFlags for BytesLiteralFlags {
     /// Return `true` if the bytestring is triple-quoted, i.e.,
     /// it begins and ends with three consecutive quote characters.
     /// For example: `b"""{bar}"""`
-    pub const fn is_triple_quoted(self) -> bool {
+    fn is_triple_quoted(self) -> bool {
         self.0.contains(BytesLiteralFlagsInner::TRIPLE_QUOTED)
     }
 
@@ -2290,7 +2268,7 @@ impl BytesLiteralFlags {
     /// used by the bytestring's opener and closer:
     /// - `b"a"` -> `QuoteStyle::Double`
     /// - `b'a'` -> `QuoteStyle::Single`
-    pub const fn quote_style(self) -> Quote {
+    fn quote_style(self) -> Quote {
         if self.0.contains(BytesLiteralFlagsInner::DOUBLE) {
             Quote::Double
         } else {
@@ -2298,25 +2276,11 @@ impl BytesLiteralFlags {
         }
     }
 
-    pub const fn is_raw_string(self) -> bool {
+    fn is_raw_string(self) -> bool {
         self.0.intersects(
             BytesLiteralFlagsInner::R_PREFIX_LOWER.union(BytesLiteralFlagsInner::R_PREFIX_UPPER),
         )
     }
-}
-
-impl StringFlags for BytesLiteralFlags {
-    fn quote_style(self) -> Quote {
-        self.quote_style()
-    }
-
-    fn is_triple_quoted(self) -> bool {
-        self.is_triple_quoted()
-    }
-
-    fn is_raw_string(self) -> bool {
-        self.is_raw_string()
-    }
 
     fn prefix(self) -> AnyStringPrefix {
         AnyStringPrefix::Bytes(self.prefix())
@@ -2471,44 +2435,6 @@ impl AnyStringFlags {
         self
     }
 
-    pub const fn prefix(self) -> AnyStringPrefix {
-        let AnyStringFlags(flags) = self;
-
-        // f-strings
-        if flags.contains(AnyStringFlagsInner::F_PREFIX) {
-            if flags.contains(AnyStringFlagsInner::R_PREFIX_LOWER) {
-                return AnyStringPrefix::Format(FStringPrefix::Raw { uppercase_r: false });
-            }
-            if flags.contains(AnyStringFlagsInner::R_PREFIX_UPPER) {
-                return AnyStringPrefix::Format(FStringPrefix::Raw { uppercase_r: true });
-            }
-            return AnyStringPrefix::Format(FStringPrefix::Regular);
-        }
-
-        // bytestrings
-        if flags.contains(AnyStringFlagsInner::B_PREFIX) {
-            if flags.contains(AnyStringFlagsInner::R_PREFIX_LOWER) {
-                return AnyStringPrefix::Bytes(ByteStringPrefix::Raw { uppercase_r: false });
-            }
-            if flags.contains(AnyStringFlagsInner::R_PREFIX_UPPER) {
-                return AnyStringPrefix::Bytes(ByteStringPrefix::Raw { uppercase_r: true });
-            }
-            return AnyStringPrefix::Bytes(ByteStringPrefix::Regular);
-        }
-
-        // all other strings
-        if flags.contains(AnyStringFlagsInner::R_PREFIX_LOWER) {
-            return AnyStringPrefix::Regular(StringLiteralPrefix::Raw { uppercase: false });
-        }
-        if flags.contains(AnyStringFlagsInner::R_PREFIX_UPPER) {
-            return AnyStringPrefix::Regular(StringLiteralPrefix::Raw { uppercase: true });
-        }
-        if flags.contains(AnyStringFlagsInner::U_PREFIX) {
-            return AnyStringPrefix::Regular(StringLiteralPrefix::Unicode);
-        }
-        AnyStringPrefix::Regular(StringLiteralPrefix::Empty)
-    }
-
     pub fn new(prefix: AnyStringPrefix, quotes: Quote, triple_quoted: bool) -> Self {
         let new = Self::default().with_prefix(prefix).with_quote_style(quotes);
         if triple_quoted {
@@ -2523,13 +2449,6 @@ impl AnyStringFlags {
         self.0.contains(AnyStringFlagsInner::U_PREFIX)
     }
 
-    /// Does the string have an `r` or `R` prefix?
-    pub const fn is_raw_string(self) -> bool {
-        self.0.intersects(
-            AnyStringFlagsInner::R_PREFIX_LOWER.union(AnyStringFlagsInner::R_PREFIX_UPPER),
-        )
-    }
-
     /// Does the string have an `f` or `F` prefix?
     pub const fn is_f_string(self) -> bool {
         self.0.contains(AnyStringFlagsInner::F_PREFIX)
@@ -2540,21 +2459,6 @@ impl AnyStringFlags {
         self.0.contains(AnyStringFlagsInner::B_PREFIX)
     }
 
-    /// Does the string use single or double quotes in its opener and closer?
-    pub const fn quote_style(self) -> Quote {
-        if self.0.contains(AnyStringFlagsInner::DOUBLE) {
-            Quote::Double
-        } else {
-            Quote::Single
-        }
-    }
-
-    /// Is the string triple-quoted, i.e.,
-    /// does it begin and end with three consecutive quote characters?
-    pub const fn is_triple_quoted(self) -> bool {
-        self.0.contains(AnyStringFlagsInner::TRIPLE_QUOTED)
-    }
-
     #[must_use]
     pub fn with_quote_style(mut self, quotes: Quote) -> Self {
         match quotes {
@@ -2572,20 +2476,64 @@ impl AnyStringFlags {
 }
 
 impl StringFlags for AnyStringFlags {
+    /// Does the string use single or double quotes in its opener and closer?
     fn quote_style(self) -> Quote {
-        self.quote_style()
+        if self.0.contains(AnyStringFlagsInner::DOUBLE) {
+            Quote::Double
+        } else {
+            Quote::Single
+        }
     }
 
+    /// Is the string triple-quoted, i.e.,
+    /// does it begin and end with three consecutive quote characters?
     fn is_triple_quoted(self) -> bool {
-        self.is_triple_quoted()
+        self.0.contains(AnyStringFlagsInner::TRIPLE_QUOTED)
     }
 
+    /// Does the string have an `r` or `R` prefix?
     fn is_raw_string(self) -> bool {
-        self.is_raw_string()
+        self.0.intersects(
+            AnyStringFlagsInner::R_PREFIX_LOWER.union(AnyStringFlagsInner::R_PREFIX_UPPER),
+        )
     }
 
     fn prefix(self) -> AnyStringPrefix {
-        self.prefix()
+        let AnyStringFlags(flags) = self;
+
+        // f-strings
+        if flags.contains(AnyStringFlagsInner::F_PREFIX) {
+            if flags.contains(AnyStringFlagsInner::R_PREFIX_LOWER) {
+                return AnyStringPrefix::Format(FStringPrefix::Raw { uppercase_r: false });
+            }
+            if flags.contains(AnyStringFlagsInner::R_PREFIX_UPPER) {
+                return AnyStringPrefix::Format(FStringPrefix::Raw { uppercase_r: true });
+            }
+            return AnyStringPrefix::Format(FStringPrefix::Regular);
+        }
+
+        // bytestrings
+        if flags.contains(AnyStringFlagsInner::B_PREFIX) {
+            if flags.contains(AnyStringFlagsInner::R_PREFIX_LOWER) {
+                return AnyStringPrefix::Bytes(ByteStringPrefix::Raw { uppercase_r: false });
+            }
+            if flags.contains(AnyStringFlagsInner::R_PREFIX_UPPER) {
+                return AnyStringPrefix::Bytes(ByteStringPrefix::Raw { uppercase_r: true });
+            }
+            return AnyStringPrefix::Bytes(ByteStringPrefix::Regular);
+        }
+
+        // all other strings
+        if flags.contains(AnyStringFlagsInner::R_PREFIX_LOWER) {
+            return AnyStringPrefix::Regular(StringLiteralPrefix::Raw { uppercase: false });
+        }
+        if flags.contains(AnyStringFlagsInner::R_PREFIX_UPPER) {
+            return AnyStringPrefix::Regular(StringLiteralPrefix::Raw { uppercase: true });
+        }
+        if flags.contains(AnyStringFlagsInner::U_PREFIX) {
+            return AnyStringPrefix::Regular(StringLiteralPrefix::Unicode);
+        }
+        AnyStringPrefix::Regular(StringLiteralPrefix::Empty)
     }
 }
 
diff --git a/crates/ruff_python_codegen/src/stylist.rs b/crates/ruff_python_codegen/src/stylist.rs
index 4511b3cf6..27516dcd5 100644
--- a/crates/ruff_python_codegen/src/stylist.rs
+++ b/crates/ruff_python_codegen/src/stylist.rs
@@ -4,7 +4,7 @@ use std::ops::Deref;
 
 use once_cell::unsync::OnceCell;
 
-use ruff_python_ast::str::Quote;
+use ruff_python_ast::{str::Quote, StringFlags};
 use ruff_python_parser::lexer::LexResult;
 use ruff_python_parser::Tok;
 use ruff_source_file::{find_newline, LineEnding, Locator};
diff --git a/crates/ruff_python_formatter/src/other/f_string.rs b/crates/ruff_python_formatter/src/other/f_string.rs
index 07248cf82..cc4859ded 100644
--- a/crates/ruff_python_formatter/src/other/f_string.rs
+++ b/crates/ruff_python_formatter/src/other/f_string.rs
@@ -1,5 +1,5 @@
 use ruff_formatter::write;
-use ruff_python_ast::{AnyStringFlags, FString};
+use ruff_python_ast::{AnyStringFlags, FString, StringFlags};
 use ruff_source_file::Locator;
 
 use crate::prelude::*;
diff --git a/crates/ruff_python_formatter/src/other/f_string_element.rs b/crates/ruff_python_formatter/src/other/f_string_element.rs
index 65b8d4512..12e653e75 100644
--- a/crates/ruff_python_formatter/src/other/f_string_element.rs
+++ b/crates/ruff_python_formatter/src/other/f_string_element.rs
@@ -3,6 +3,7 @@ use std::borrow::Cow;
 use ruff_formatter::{format_args, write, Buffer, RemoveSoftLinesBuffer};
 use ruff_python_ast::{
     ConversionFlag, Expr, FStringElement, FStringExpressionElement, FStringLiteralElement,
+    StringFlags,
 };
 use ruff_text_size::Ranged;
 
diff --git a/crates/ruff_python_formatter/src/string/any.rs b/crates/ruff_python_formatter/src/string/any.rs
index 011952ce4..b621027c2 100644
--- a/crates/ruff_python_formatter/src/string/any.rs
+++ b/crates/ruff_python_formatter/src/string/any.rs
@@ -4,7 +4,7 @@ use memchr::memchr2;
 
 use ruff_python_ast::{
     self as ast, AnyNodeRef, AnyStringFlags, Expr, ExprBytesLiteral, ExprFString,
-    ExprStringLiteral, ExpressionRef, StringLiteral,
+    ExprStringLiteral, ExpressionRef, StringFlags, StringLiteral,
 };
 use ruff_source_file::Locator;
 use ruff_text_size::{Ranged, TextRange};
diff --git a/crates/ruff_python_formatter/src/string/docstring.rs b/crates/ruff_python_formatter/src/string/docstring.rs
index 4ea431abe..6aefad2a1 100644
--- a/crates/ruff_python_formatter/src/string/docstring.rs
+++ b/crates/ruff_python_formatter/src/string/docstring.rs
@@ -8,7 +8,7 @@ use std::{borrow::Cow, collections::VecDeque};
 use itertools::Itertools;
 
 use ruff_formatter::printer::SourceMapGeneration;
-use ruff_python_ast::str::Quote;
+use ruff_python_ast::{str::Quote, StringFlags};
 use ruff_python_parser::ParseError;
 use {once_cell::sync::Lazy, regex::Regex};
 use {
diff --git a/crates/ruff_python_formatter/src/string/normalize.rs b/crates/ruff_python_formatter/src/string/normalize.rs
index 43a47e3fe..f8ab27c53 100644
--- a/crates/ruff_python_formatter/src/string/normalize.rs
+++ b/crates/ruff_python_formatter/src/string/normalize.rs
@@ -2,7 +2,7 @@ use std::borrow::Cow;
 use std::iter::FusedIterator;
 
 use ruff_formatter::FormatContext;
-use ruff_python_ast::{str::Quote, AnyStringFlags};
+use ruff_python_ast::{str::Quote, AnyStringFlags, StringFlags};
 use ruff_source_file::Locator;
 use ruff_text_size::{Ranged, TextRange};
 
diff --git a/crates/ruff_python_index/src/multiline_ranges.rs b/crates/ruff_python_index/src/multiline_ranges.rs
index fd5dd1281..8043929aa 100644
--- a/crates/ruff_python_index/src/multiline_ranges.rs
+++ b/crates/ruff_python_index/src/multiline_ranges.rs
@@ -1,3 +1,4 @@
+use ruff_python_ast::StringFlags;
 use ruff_python_parser::Tok;
 use ruff_text_size::TextRange;
 
diff --git a/crates/ruff_python_parser/src/lexer.rs b/crates/ruff_python_parser/src/lexer.rs
index 71866a036..34d572204 100644
--- a/crates/ruff_python_parser/src/lexer.rs
+++ b/crates/ruff_python_parser/src/lexer.rs
@@ -37,7 +37,7 @@ use unicode_normalization::UnicodeNormalization;
 use ruff_python_ast::{
     str::Quote,
     str_prefix::{AnyStringPrefix, FStringPrefix},
-    AnyStringFlags, Int, IpyEscapeKind,
+    AnyStringFlags, Int, IpyEscapeKind, StringFlags,
 };
 use ruff_text_size::{TextLen, TextRange, TextSize};
 
diff --git a/crates/ruff_python_parser/src/lexer/fstring.rs b/crates/ruff_python_parser/src/lexer/fstring.rs
index c466faee1..16dae1222 100644
--- a/crates/ruff_python_parser/src/lexer/fstring.rs
+++ b/crates/ruff_python_parser/src/lexer/fstring.rs
@@ -36,7 +36,7 @@ impl FStringContext {
     }
 
     /// Returns the quote character for the current f-string.
-    pub(crate) const fn quote_char(&self) -> char {
+    pub(crate) fn quote_char(&self) -> char {
         self.flags.quote_style().as_char()
     }
 
@@ -56,7 +56,7 @@ impl FStringContext {
     }
 
     /// Returns `true` if the current f-string is a triple-quoted f-string.
-    pub(crate) const fn is_triple_quoted(&self) -> bool {
+    pub(crate) fn is_triple_quoted(&self) -> bool {
         self.flags.is_triple_quoted()
     }
 

```

</details>

---

_@MichaReiser reviewed on 2024-05-27 14:19_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1523 on 2024-05-27 14:19_

Bringing the trait into the scope is how rust works. So that's totally fine and kind of what you want. But I also struggled with this at the beginning. 

It is possible to define both methods to avoid the need for importing, but I think we then lose the consistency benefit of having a trait in the first place, in which case I would prefer not to have the trait.

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1523 on 2024-05-27 14:21_

That makes sense, thanks.

---

_@AlexWaygood reviewed on 2024-05-27 14:21_

---

_Comment by @AlexWaygood on 2024-05-27 14:38_

I also wasn't sure about this initially, but I'm feeling a lot better about it after addressing your review. It's now much less additional code, and I think having a consistent interface for all these types is a nice win. Planning to land once CI passes.

---

_Merged by @AlexWaygood on 2024-05-27 15:02_

---

_Closed by @AlexWaygood on 2024-05-27 15:02_

---

_Branch deleted on 2024-05-27 15:02_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:1354 on 2024-05-28 04:16_

Nice work! I had something similar in mind but I thought of using associated type for prefix:

```rs
pub trait StringFlags: Copy {
	type Prefix;

	fn prefix(self) -> Self::Prefix;
}
```

---

_@dhruvmanila reviewed on 2024-05-28 04:16_

---
