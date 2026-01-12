```yaml
number: 9058
title: Split string formatting to individual nodes
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/split-string-formatting
created_at: 2023-12-08T20:38:28Z
updated_at: 2023-12-14T18:55:11Z
url: https://github.com/astral-sh/ruff/pull/9058
synced_at: 2026-01-12T15:55:27Z
```

# Split string formatting to individual nodes

---

_@dhruvmanila_

This PR splits the string formatting code in the formatter to be handled by the respective nodes.

Previously, the string formatting was done through a single `FormatString` interface. Now, the nodes themselves are responsible for formatting.

The following changes were made:
1. Remove `StringLayout::ImplicitStringConcatenationInBinaryLike` and inline the call to `FormatStringContinuation`. After the refactor, the binary like formatting would delegate to `FormatString` which would then delegate to `FormatStringContinuation`. This removes the intermediary steps.
2. Add formatter implementation for `FStringPart` which delegates it to the respective string literal or f-string node.
3. Add `ExprStringLiteralKind` which is either `String` or `Docstring`. If it's a docstring variant, then the string expression would not be implicitly concatenated. This is guaranteed by the `DocstringStmt::try_from_expression` constructor.
4. Add `StringLiteralKind` which is either a `String`, `Docstring` or `InImplicitlyConcatenatedFString`. The last variant is for when the string literal is  implicitly concatenated with an f-string (`"foo" f"bar {x}"`).
5. Remove `FormatString`.
6. Extract the f-string quote detection as a standalone function which is public to the crate. This is used to detect the quote to be used for an f-string at the expression level (`ExprFString` or `FormatStringContinuation`).


### Formatter ecosystem result

**This PR**

| project        | similarity index  | total files       | changed files     |
|----------------|------------------:|------------------:|------------------:|
| cpython        |           0.75804 |              1799 |              1648 |
| django         |           0.99984 |              2772 |                34 |
| home-assistant |           0.99955 |             10596 |               214 |
| poetry         |           0.99905 |               321 |                15 |
| transformers   |           0.99967 |              2657 |               324 |
| twine          |           1.00000 |                33 |                 0 |
| typeshed       |           0.99980 |              3669 |                18 |
| warehouse      |           0.99976 |               654 |                14 |
| zulip          |           0.99958 |              1459 |                36 |

**main**

| project        | similarity index  | total files       | changed files     |
|----------------|------------------:|------------------:|------------------:|
| cpython        |           0.75804 |              1799 |              1648 |
| django         |           0.99984 |              2772 |                34 |
| home-assistant |           0.99955 |             10596 |               214 |
| poetry         |           0.99905 |               321 |                15 |
| transformers   |           0.99967 |              2657 |               324 |
| twine          |           1.00000 |                33 |                 0 |
| typeshed       |           0.99980 |              3669 |                18 |
| warehouse      |           0.99976 |               654 |                14 |
| zulip          |           0.99958 |              1459 |                36 |

---

_Comment by @github-actions[bot] on 2023-12-08 20:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @dhruvmanila on 2023-12-12 01:09_

Current dependencies on/for this PR:
* `main`
  * **PR #9111** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9111?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #9058** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9058?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/9058?utm_source=stack-comment).

---

_Renamed from "WIP: Initial attempt to split string formatting" to "Split string formatting to individual nodes" by @dhruvmanila on 2023-12-13 03:01_

---

_Label `internal` added by @dhruvmanila on 2023-12-13 03:01_

---

_Marked ready for review by @dhruvmanila on 2023-12-13 06:34_

---

_Review requested from @MichaReiser by @dhruvmanila on 2023-12-13 06:34_

---

_Review requested from @konstin by @dhruvmanila on 2023-12-13 06:34_

---

_Comment by @MichaReiser on 2023-12-13 06:56_

Would you mind including a comparision of the ` ./scripts/formatter_ecosystem_checks.s` output between main and your PR, just to make sure that this is indeed backwards compatible.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_string_literal.rs`:16 on 2023-12-13 07:02_

The term `context` is a bit confusing becaue it makes me think of `PyFormatContext` (which I first assumed this is). Can we give this a less generic name? Or call it `StringOptions` (see `FormatRuleWithOptions`)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/mod.rs`:147 on 2023-12-13 07:09_

As recommended in `FormatStringLiteral`. We can replace `StringContext` with `Quoting` when we use a `StringLiteralLayout` as the option of `FormatStringLiteral`.


---

_@konstin approved on 2023-12-13 08:50_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/binary_like.rs`:398 on 2023-12-13 09:22_

It's not immediately obvious why it formats the string without the enclosing group. I think it would help if we extend the comment saying: Call `FormatStringContinuation` directly to avoid formatting the string with the enclosing...

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_f_string.rs`:17 on 2023-12-13 09:25_

`with_options` seems to never be called on `ExprFString`
```suggestion

```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_string_literal.rs`:33 on 2023-12-13 09:29_

The naming here is a bit confusing: 

* We're inside the `ExprStringLiteral` (...literal)
* But the `value` is now called `string_literal` but it is the part

I think the rename that I recommended on the new AST node PR would help here as well. 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/bytes_literal.rs`:13 on 2023-12-13 09:32_

Nit: You may want to inline these variables (or extract `f.options().quote_style(())`) too.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/string_literal.rs`:11 on 2023-12-13 09:43_

Nit: I think I would limit the `FormatStringLiteral` to accept a layout which is either `Docstring` or `Default` to hide the string internals as much as possible.

For example, the `quoting` of `Docstring` is never used but it seems as if it is passed down. 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_string.rs`:10 on 2023-12-13 09:49_

Using `FormatRuleWithOptions` here has the downside that the compiler doesn't warn you when you forget to call `.with_options`, in which case the formatter simply assumes the `StringContext::default`. 

I think it might be better to either set `context` to `Option` and add an assertion that throws if it is `None` or not use `FormatRule` (and `AsFormat` etc) and instead require an explicit construction of `FormatFString`

```rust
write!(f, [FormatFString { string: part, context: StringContext }])
```

It may also be useful to replace `StringContext` with quoting, because that seems to be all what is needed in here (f strings can't be docstrings)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_string_part.rs`:23 on 2023-12-13 09:50_

The same as for `FormatString`: Consider creating `FormatFStringPart` manually instead of using `AsFormat` because passing the `context` is required. Consider passing `Quoting` directly, because the context is never needed for fstrings.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/mod.rs`:462 on 2023-12-13 09:53_

It seems that not all these methods need to be `pub(crate)`. Please double check which ones are necessary to be visible outside of this module.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/mod.rs`:224 on 2023-12-13 09:55_

I think it's better to pass the context in the constructor because failing to call `with_context` uses the default context which may not use the right quotes. 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/mod.rs`:195 on 2023-12-13 09:56_

I think it's better to pass the context in the constructor because failing to call `with_context` uses the default context which may not use the right quotes. 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/mod.rs`:151 on 2023-12-13 09:57_

I think all these methods can be made `pub(crate)`

---

_@MichaReiser requested changes on 2023-12-13 10:03_

Moving the formatting into the corresponding nodes helps to untangle some of the complexity. 

It overall looks good, but there are some places where we can reduce the complexity by limiting the option types further.

My main feedback is not specifically related to this PR but to the string parts refactor. 

I continue to find it extremelly difficult to keep the different string nodes apart. because we have `ExprStringLiteral` and `StringLiteral`. I made a naming recommendation on your most recent AST refactor and I believe that implementing it could greatly help with naming (including downstream logic like the formatting code here). Doing this renaming sooner rather than later will ensure that downstream crates use the terminology consistently. 

My other feedback related to this refactor is that I find the `StringLiteralValue` concept difficult to understand because it seems to be mainly an implementation detail. Reducing that additional concept (from the already complicated string nodes) by making `value` and the `StringLiteralValue` struct private, and moving all `StringLiteralValue` methods to `StringLiteralExpression` might help further reduce the complexity of our string nodes (at least the perceived complexity)

I don't expect us to do this refactor as part of this PR but I think it will be worthwile to improve the naming of the string nodes, because I fail to understand the structure even after having looked up their definitions a couple of time.

---

_@dhruvmanila reviewed on 2023-12-13 18:16_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/other/bytes_literal.rs`:13 on 2023-12-13 18:16_

I've inlined the `docstring` call but the locator is being used in multiple locations.

---

_@dhruvmanila reviewed on 2023-12-13 18:20_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/other/string_literal.rs`:11 on 2023-12-13 18:20_

Good point. I thought so as well but then I realized that we need the quoting information when the literal is a part of an f-string which is implicitly concatenated. For example,

```python
"foo" f"bar {x}"
```

Here, the top level node is `ExprFString` which delegates the formatting to the individual parts which are `StringLiteral` (`"foo"`) and `FString` (`f"bar {x}"`). The f-string quoting logic takes place in `ExprFString` and is being passed to all the parts including `StringLiteral`.

I hope this clears up any doubts but if there's any other confusion, please do ask.

---

_@dhruvmanila reviewed on 2023-12-13 18:25_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/other/f_string_part.rs`:23 on 2023-12-13 18:25_

I don't think it'll work for `FormatFStringPart` due to it being required in `StringLiteral`. Refer https://github.com/astral-sh/ruff/pull/9058#discussion_r1425727018

---

_@dhruvmanila reviewed on 2023-12-13 18:38_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/other/string_literal.rs`:11 on 2023-12-13 18:38_

This is the main reason to introduce `StringContext` otherwise we don't really need it.

---

_@dhruvmanila reviewed on 2023-12-13 18:40_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/other/f_string.rs`:10 on 2023-12-13 18:40_

This is beneficial but not completely in the way you expected mainly because of my comment above (https://github.com/astral-sh/ruff/pull/9058#discussion_r1425727018).

---

_@dhruvmanila reviewed on 2023-12-13 18:49_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/string/mod.rs`:224 on 2023-12-13 18:49_

There are calls to `FormatStringContinuation::new` which uses the default context. @MichaReiser Should we explicitly pass in the default context in those cases? I'm fine with either.

---

_@dhruvmanila reviewed on 2023-12-13 18:57_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/expression/expr_string_literal.rs`:33 on 2023-12-13 18:57_

Yeah, I agree. I've started off with the renaming locally and will put up a PR today. There's some thinking to do with the f-string side of the names.

---

_@dhruvmanila reviewed on 2023-12-13 19:02_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/other/f_string.rs`:10 on 2023-12-13 19:02_

I've made this change and updated the `generate.py` script to ignore generating impls for `FString` node. Do you think it would be useful to move the file from `other/f_string.rs` to `string/f_string.rs` crate?

---

_Comment by @dhruvmanila on 2023-12-13 19:10_

Thanks for the detailed review. I've made most of the requested changes:
1. Rename `StringContext` to `StringOptions`, `with_context` to `with_options`.
2. Limit the visibility of various structs and methods as per it's usage.
3. Remove `impl FormatRuleWithOptions for FormatExprFString`.
4. Remove `AsFormat` for `FString`, update `FormatFString` struct with a `new` method which requires the f-string node and the necessary quoting.

Open points:
1. Explicitly passing the `StringContext` to `FormatStringContinuation` constructor - in some cases, the call is made without the `with_context` method chain.
2. The requirement of `StringContext` mainly because the `StringLiteral` formatting requires both the quoting and layout information. The quoting comes when a string literal is part of an implicitly concatenated f-string while the layout comes when it's a docstring.

---

_@dhruvmanila reviewed on 2023-12-13 20:35_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/expression/expr_string_literal.rs`:33 on 2023-12-13 20:35_

https://github.com/astral-sh/ruff/pull/9120

---

_Comment by @dhruvmanila on 2023-12-13 20:47_

> My other feedback related to this refactor is that I find the `StringLiteralValue` concept difficult to understand because it seems to be mainly an implementation detail. Reducing that additional concept (from the already complicated string nodes) by making `value` and the `StringLiteralValue` struct private, and moving all `StringLiteralValue` methods to `StringLiteralExpression` might help further reduce the complexity of our string nodes (at least the perceived complexity)

Yeah, thanks for providing your perspective on this. With the AST rename PR, I'm able to understand why this might be so. I'll probably do a follow-up next week to merge `*Value` and make the `value` field private.

---

_@MichaReiser reviewed on 2023-12-14 00:22_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/mod.rs`:224 on 2023-12-14 00:22_

To me this mainly depends if the default context is guaranteed to do the right thing or if forgetting to call `with_context` may assume the wrong quotes and mess things up. In other words: Is calling `with_context` optional (`with_context` might be fine) or is it always required (constructor argument). I think we should always provide it because it can't know the right quotes, can it?

Also, implicit concatenated strings can never be docstrings, right? We should then change the `FormatStringContinuation` to only take `quoting` as an option to encode this information in the types.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/binary_like.rs`:401 on 2023-12-14 00:37_

Is it save to omit `Quoting` here? Can `string_constent` be an `f-string` for which quotes need to be determined first and be preserved? I recommend making `quoting` a constructor argument if omitting it can lead to invalid results.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_string_part.rs`:34 on 2023-12-14 00:42_

F-string parts can never be docstrings. Only passing `quoting` encodes this invariant better
```suggestion
#[derive(Default)]
pub struct FormatFStringPart {
    quoting: Quoting,
}

impl FormatRuleWithOptions<FStringPart, PyFormatContext<'_>> for FormatFStringPart {
    type Options = Quoting;

    fn with_options(mut self, options: Self::Options) -> Self {
        self.quoting = options;
        self
    }
}

impl FormatRule<FStringPart, PyFormatContext<'_>> for FormatFStringPart {
    fn fmt(&self, item: &FStringPart, f: &mut PyFormatter) -> FormatResult<()> {
        match item {
            FStringPart::Literal(string_literal) => {
                string_literal.format().with_options(StringOptions::default().with_quoting(self.quoting)).fmt(f)
            }
            FStringPart::FString(f_string) => {
                FormatFString::new(f_string, self.quoting).fmt(f)
            }
        }
    }
}

```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_string_part.rs`:49 on 2023-12-14 00:44_

I believe calling `FormatFStringPart` should always require specifing `quoting`. If that assumption is correct, I recommend removing the `AsFormat` and `IntoFormat` implementations and require passing `quoting` as part of the constructor.

Or make `quoting` an `Option` and add an assertion that asserts that `quoting` is set (using `expect`).

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_string_part.rs`:39 on 2023-12-14 00:51_

It might be sufficient to pass `quoting` instead of `StringOptions` because FStrings can never be docstrings


```suggestion
pub struct FormatFStringPart {
    quoting: Quoting,
}

impl FormatRuleWithOptions<FStringPart, PyFormatContext<'_>> for FormatFStringPart {
    type Options = Quoting;

    fn with_options(mut self, options: Self::Options) -> Self {
        self.quoting = options;
        self
    }
}

impl FormatRule<FStringPart, PyFormatContext<'_>> for FormatFStringPart {
    fn fmt(&self, item: &FStringPart, f: &mut PyFormatter) -> FormatResult<()> {
        match item {
            FStringPart::Literal(string_literal) => {
                string_literal.format().with_options(StringOptions::default().with_quoting(self.quoting)).fmt(f)
            }
            FStringPart::FString(f_string) => {
                FormatFString::new(f_string, self.quoting).fmt(f)
            }
        }
    }
}
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/mod.rs`:203 on 2023-12-14 02:13_

We should be able to change this to `quoting` but it requires changing `AnyStringPart` too:

Change the `StringLiteral` and `FString` variants of `AnyStringPart` and store the string literal kind and the quoting in the variants:

```rust
enum AnyStringPart<'a> {
    String {
        literal: &'a ast::StringLiteral,
        kind: StringLiteralKind,
    },
    Bytes(&'a ast::BytesLiteral),
    FString {
        string: &'a ast::FString,
        quoting: Quoting,
    },
}
```

And then implement `Format` directly on `AnyStringPart`. 

---

_@MichaReiser approved on 2023-12-14 02:27_

This is looking good to me but I think it would be helpful to avoid using `StringOptions` (or remove it entirely) and instead use more specific option types. I'm saying this because I struggled hard to understand which combinations of `StringOptions` are possible in the different formatting functions. 

I commented on the PR where I think we can narrow the options but you can also use the following diff if you want all changes applied (requires some cleanup because I didn't always rename fields to match the type of the options). 

```patch
Index: crates/ruff_python_formatter/src/expression/expr_f_string.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_python_formatter/src/expression/expr_f_string.rs b/crates/ruff_python_formatter/src/expression/expr_f_string.rs
--- a/crates/ruff_python_formatter/src/expression/expr_f_string.rs	(revision Staged)
+++ b/crates/ruff_python_formatter/src/expression/expr_f_string.rs	(date 1702518874243)
@@ -9,20 +9,19 @@
     in_parentheses_only_group, NeedsParentheses, OptionalParentheses,
 };
 use crate::prelude::*;
-use crate::string::{AnyString, FormatStringContinuation, Quoting, StringOptions};
+use crate::string::{AnyString, FormatStringContinuation, Quoting};
 
 #[derive(Default)]
 pub struct FormatExprFString;
 
 impl FormatNodeRule<ExprFString> for FormatExprFString {
     fn fmt_fields(&self, item: &ExprFString, f: &mut PyFormatter) -> FormatResult<()> {
-        let options =
-            StringOptions::default().with_quoting(f_string_quoting(item, &f.context().locator()));
+        let quoting = f_string_quoting(item, &f.context().locator());
 
         match item.value.as_slice() {
-            [f_string_part] => f_string_part.format().with_options(options).fmt(f),
+            [f_string_part] => f_string_part.format().with_options(quoting).fmt(f),
             _ => in_parentheses_only_group(
-                &FormatStringContinuation::new(&AnyString::FString(item)).with_options(options),
+                &FormatStringContinuation::new(&AnyString::FString(item)).with_options(quoting),
             )
             .fmt(f),
         }
Index: crates/ruff_python_formatter/src/expression/expr_string_literal.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_python_formatter/src/expression/expr_string_literal.rs b/crates/ruff_python_formatter/src/expression/expr_string_literal.rs
--- a/crates/ruff_python_formatter/src/expression/expr_string_literal.rs	(revision Staged)
+++ b/crates/ruff_python_formatter/src/expression/expr_string_literal.rs	(date 1702519544384)
@@ -6,18 +6,37 @@
 use crate::expression::parentheses::{
     in_parentheses_only_group, NeedsParentheses, OptionalParentheses,
 };
+use crate::other::string_literal::StringLiteralKind;
 use crate::prelude::*;
-use crate::string::{
-    AnyString, FormatStringContinuation, StringOptions, StringPrefix, StringQuotes,
-};
+use crate::string::{AnyString, FormatStringContinuation, Quoting, StringPrefix, StringQuotes};
 
 #[derive(Default)]
 pub struct FormatExprStringLiteral {
-    options: StringOptions,
+    options: ExprStringLiteralKind,
+}
+
+#[derive(Default, Copy, Clone, Debug)]
+pub enum ExprStringLiteralKind {
+    #[default]
+    String,
+    Docstring,
+}
+
+impl ExprStringLiteralKind {
+    const fn string_literal_kind(self) -> StringLiteralKind {
+        match self {
+            ExprStringLiteralKind::String => StringLiteralKind::String,
+            ExprStringLiteralKind::Docstring => StringLiteralKind::Docstring,
+        }
+    }
+
+    const fn is_docstring(self) -> bool {
+        matches!(self, Self::Docstring)
+    }
 }
 
 impl FormatRuleWithOptions<ExprStringLiteral, PyFormatContext<'_>> for FormatExprStringLiteral {
-    type Options = StringOptions;
+    type Options = ExprStringLiteralKind;
 
     fn with_options(mut self, options: Self::Options) -> Self {
         self.options = options;
@@ -30,11 +49,15 @@
         let ExprStringLiteral { value, .. } = item;
 
         match value.as_slice() {
-            [string_literal] => string_literal.format().with_options(self.options).fmt(f),
-            _ => in_parentheses_only_group(
-                &FormatStringContinuation::new(&AnyString::String(item)).with_options(self.options),
-            )
-            .fmt(f),
+            [string_literal] => string_literal
+                .format()
+                .with_options(self.options.string_literal_kind())
+                .fmt(f),
+            _ => {
+                assert!(!self.options.is_docstring());
+                in_parentheses_only_group(&FormatStringContinuation::new(&AnyString::String(item)))
+                    .fmt(f)
+            }
         }
     }
 
Index: crates/ruff_python_formatter/src/other/f_string_part.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_python_formatter/src/other/f_string_part.rs b/crates/ruff_python_formatter/src/other/f_string_part.rs
--- a/crates/ruff_python_formatter/src/other/f_string_part.rs	(revision Staged)
+++ b/crates/ruff_python_formatter/src/other/f_string_part.rs	(date 1702519544305)
@@ -2,16 +2,17 @@
 use ruff_python_ast::FStringPart;
 
 use crate::other::f_string::FormatFString;
+use crate::other::string_literal::StringLiteralKind;
 use crate::prelude::*;
-use crate::string::StringOptions;
+use crate::string::Quoting;
 
 #[derive(Default)]
 pub struct FormatFStringPart {
-    options: StringOptions,
+    options: Quoting,
 }
 
 impl FormatRuleWithOptions<FStringPart, PyFormatContext<'_>> for FormatFStringPart {
-    type Options = StringOptions;
+    type Options = Quoting;
 
     fn with_options(mut self, options: Self::Options) -> Self {
         self.options = options;
@@ -22,12 +23,11 @@
 impl FormatRule<FStringPart, PyFormatContext<'_>> for FormatFStringPart {
     fn fmt(&self, item: &FStringPart, f: &mut PyFormatter) -> FormatResult<()> {
         match item {
-            FStringPart::Literal(string_literal) => {
-                string_literal.format().with_options(self.options).fmt(f)
-            }
-            FStringPart::FString(f_string) => {
-                FormatFString::new(f_string, self.options.quoting()).fmt(f)
-            }
+            FStringPart::Literal(string_literal) => string_literal
+                .format()
+                .with_options(StringLiteralKind::InFString(self.options))
+                .fmt(f),
+            FStringPart::FString(f_string) => FormatFString::new(f_string, self.options).fmt(f),
         }
     }
 }
Index: crates/ruff_python_formatter/src/other/string_literal.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_python_formatter/src/other/string_literal.rs b/crates/ruff_python_formatter/src/other/string_literal.rs
--- a/crates/ruff_python_formatter/src/other/string_literal.rs	(revision Staged)
+++ b/crates/ruff_python_formatter/src/other/string_literal.rs	(date 1702519695435)
@@ -3,19 +3,40 @@
 use ruff_text_size::Ranged;
 
 use crate::prelude::*;
-use crate::string::{docstring, StringOptions, StringPart};
+use crate::string::{docstring, Quoting, StringPart};
 use crate::QuoteStyle;
 
 #[derive(Default)]
 pub struct FormatStringLiteral {
-    options: StringOptions,
+    layout: StringLiteralKind,
+}
+
+#[derive(Copy, Clone, Debug, Default)]
+pub enum StringLiteralKind {
+    #[default]
+    String,
+    Docstring,
+    InFString(Quoting),
+}
+
+impl StringLiteralKind {
+    pub(crate) const fn is_docstring(self) -> bool {
+        matches!(self, StringLiteralKind::Docstring)
+    }
+
+    fn quoting(self) -> Quoting {
+        match self {
+            StringLiteralKind::String | StringLiteralKind::Docstring => Quoting::CanChange,
+            StringLiteralKind::InFString(quoting) => quoting,
+        }
+    }
 }
 
 impl FormatRuleWithOptions<StringLiteral, PyFormatContext<'_>> for FormatStringLiteral {
-    type Options = StringOptions;
+    type Options = StringLiteralKind;
 
     fn with_options(mut self, options: Self::Options) -> Self {
-        self.options = options;
+        self.layout = options;
         self
     }
 }
@@ -25,24 +46,24 @@
         let locator = f.context().locator();
         let parent_docstring_quote_style = f.context().docstring();
 
-        let quote_style = if self.options.is_docstring() {
+        let quote_style = match self.layout {
             // Per PEP 8 and PEP 257, always prefer double quotes for docstrings
-            QuoteStyle::Double
-        } else {
-            f.options().quote_style()
+            StringLiteralKind::Docstring => QuoteStyle::Double,
+            StringLiteralKind::String | StringLiteralKind::InFString(_) => {
+                f.options().quote_style()
+            }
         };
 
         let normalized = StringPart::from_source(item.range(), &locator).normalize(
-            self.options.quoting(),
+            self.layout.quoting(),
             &locator,
             quote_style,
             parent_docstring_quote_style,
         );
 
-        if self.options.is_docstring() {
-            docstring::format(&normalized, f)
-        } else {
-            normalized.fmt(f)
+        match self.layout {
+            StringLiteralKind::Docstring => docstring::format(&normalized, f),
+            StringLiteralKind::String | StringLiteralKind::InFString(_) => normalized.fmt(f),
         }
     }
 }
Index: crates/ruff_python_formatter/src/statement/suite.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_python_formatter/src/statement/suite.rs b/crates/ruff_python_formatter/src/statement/suite.rs
--- a/crates/ruff_python_formatter/src/statement/suite.rs	(revision Staged)
+++ b/crates/ruff_python_formatter/src/statement/suite.rs	(date 1702519544227)
@@ -9,9 +9,10 @@
     leading_comments, trailing_comments, Comments, LeadingDanglingTrailingComments,
 };
 use crate::context::{NodeLevel, TopLevelStatementPosition, WithIndentLevel, WithNodeLevel};
+use crate::expression::expr_string_literal::ExprStringLiteralKind;
+use crate::other::string_literal::StringLiteralKind;
 use crate::prelude::*;
 use crate::statement::stmt_expr::FormatStmtExpr;
-use crate::string::StringOptions;
 use crate::verbatim::{
     suppressed_node, write_suppressed_statements_starting_with_leading_comment,
     write_suppressed_statements_starting_with_trailing_comment,
@@ -609,7 +610,7 @@
                     leading_comments(node_comments.leading),
                     string_literal
                         .format()
-                        .with_options(StringOptions::docstring()),
+                        .with_options(ExprStringLiteralKind::Docstring),
                 ]
             )?;
 
Index: crates/ruff_python_formatter/src/string/mod.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_python_formatter/src/string/mod.rs b/crates/ruff_python_formatter/src/string/mod.rs
--- a/crates/ruff_python_formatter/src/string/mod.rs	(revision Staged)
+++ b/crates/ruff_python_formatter/src/string/mod.rs	(date 1702519544492)
@@ -13,6 +13,7 @@
 use crate::comments::{leading_comments, trailing_comments};
 use crate::expression::parentheses::in_parentheses_only_soft_line_break_or_space;
 use crate::other::f_string::FormatFString;
+use crate::other::string_literal::StringLiteralKind;
 use crate::prelude::*;
 use crate::QuoteStyle;
 
@@ -47,21 +48,29 @@
         }
     }
 
-    fn parts(&self) -> Vec<AnyStringPart<'a>> {
+    fn parts(&self, quoting: Quoting) -> Vec<AnyStringPart<'a>> {
         match self {
-            Self::String(ExprStringLiteral { value, .. }) => {
-                value.iter().map(AnyStringPart::String).collect()
-            }
+            Self::String(ExprStringLiteral { value, .. }) => value
+                .iter()
+                .map(|part| AnyStringPart::String {
+                    literal: part,
+                    layout: StringLiteralKind::String,
+                })
+                .collect(),
             Self::Bytes(ExprBytesLiteral { value, .. }) => {
                 value.iter().map(AnyStringPart::Bytes).collect()
             }
             Self::FString(ExprFString { value, .. }) => value
                 .iter()
                 .map(|f_string_part| match f_string_part {
-                    ast::FStringPart::Literal(string_literal) => {
-                        AnyStringPart::String(string_literal)
-                    }
-                    ast::FStringPart::FString(f_string) => AnyStringPart::FString(f_string),
+                    ast::FStringPart::Literal(string_literal) => AnyStringPart::String {
+                        literal: string_literal,
+                        layout: StringLiteralKind::InFString(quoting),
+                    },
+                    ast::FStringPart::FString(f_string) => AnyStringPart::FString {
+                        string: f_string,
+                        quoting,
+                    },
                 })
                 .collect(),
         }
@@ -100,17 +109,23 @@
 
 #[derive(Clone, Debug)]
 enum AnyStringPart<'a> {
-    String(&'a ast::StringLiteral),
+    String {
+        literal: &'a ast::StringLiteral,
+        layout: StringLiteralKind,
+    },
     Bytes(&'a ast::BytesLiteral),
-    FString(&'a ast::FString),
+    FString {
+        string: &'a ast::FString,
+        quoting: Quoting,
+    },
 }
 
 impl<'a> From<&AnyStringPart<'a>> for AnyNodeRef<'a> {
     fn from(value: &AnyStringPart<'a>) -> Self {
         match value {
-            AnyStringPart::String(part) => AnyNodeRef::StringLiteral(part),
+            AnyStringPart::String { literal, .. } => AnyNodeRef::StringLiteral(literal),
             AnyStringPart::Bytes(part) => AnyNodeRef::BytesLiteral(part),
-            AnyStringPart::FString(part) => AnyNodeRef::FString(part),
+            AnyStringPart::FString { string, .. } => AnyNodeRef::FString(string),
         }
     }
 }
@@ -118,101 +133,49 @@
 impl Ranged for AnyStringPart<'_> {
     fn range(&self) -> TextRange {
         match self {
-            Self::String(part) => part.range(),
+            Self::String { literal, .. } => literal.range(),
             Self::Bytes(part) => part.range(),
-            Self::FString(part) => part.range(),
+            Self::FString { string, .. } => string.range(),
+        }
+    }
+}
+
+impl Format<PyFormatContext<'_>> for AnyStringPart<'_> {
+    fn fmt(&self, f: &mut PyFormatter) -> FormatResult<()> {
+        match self {
+            AnyStringPart::String { literal, layout } => {
+                literal.format().with_options(*layout).fmt(f)
+            }
+            AnyStringPart::Bytes(bytes_literal) => bytes_literal.format().fmt(f),
+            AnyStringPart::FString { string, quoting } => {
+                FormatFString::new(string, *quoting).fmt(f)
+            }
         }
     }
 }
 
 #[derive(Copy, Clone, Debug, Default)]
-pub(crate) enum Quoting {
+pub enum Quoting {
     #[default]
     CanChange,
     Preserve,
 }
 
-#[derive(Default, Copy, Clone, Debug)]
-pub(crate) enum StringLayout {
-    #[default]
-    Default,
-    DocString,
-}
-
-/// Resolved options for formatting any kind of string. This can be either a string,
-/// bytes or f-string.
-#[derive(Copy, Clone, Debug, Default)]
-pub struct StringOptions {
-    quoting: Quoting,
-    layout: StringLayout,
-}
-
-impl StringOptions {
-    /// Creates a new context with the docstring layout.
-    pub(crate) fn docstring() -> Self {
-        Self {
-            layout: StringLayout::DocString,
-            ..Self::default()
-        }
-    }
-
-    /// Returns a new context with the given [`Quoting`] style.
-    pub(crate) const fn with_quoting(mut self, quoting: Quoting) -> Self {
-        self.quoting = quoting;
-        self
-    }
-
-    /// Returns the [`Quoting`] style to use for the string.
-    pub(crate) const fn quoting(self) -> Quoting {
-        self.quoting
-    }
-
-    /// Returns `true` if the string is a docstring.
-    pub(crate) const fn is_docstring(self) -> bool {
-        matches!(self.layout, StringLayout::DocString)
-    }
-}
-
-struct FormatStringPart<'a> {
-    part: &'a AnyStringPart<'a>,
-    options: StringOptions,
-}
-
-impl<'a> FormatStringPart<'a> {
-    fn new(part: &'a AnyStringPart<'a>, options: StringOptions) -> Self {
-        Self { part, options }
-    }
-}
-
-impl Format<PyFormatContext<'_>> for FormatStringPart<'_> {
-    fn fmt(&self, f: &mut PyFormatter) -> FormatResult<()> {
-        match self.part {
-            AnyStringPart::String(string_literal) => {
-                string_literal.format().with_options(self.options).fmt(f)
-            }
-            AnyStringPart::Bytes(bytes_literal) => bytes_literal.format().fmt(f),
-            AnyStringPart::FString(f_string) => {
-                FormatFString::new(f_string, self.options.quoting()).fmt(f)
-            }
-        }
-    }
-}
-
 pub(crate) struct FormatStringContinuation<'a> {
     string: &'a AnyString<'a>,
-    options: StringOptions,
+    quoting: Quoting,
 }
 
 impl<'a> FormatStringContinuation<'a> {
     pub(crate) fn new(string: &'a AnyString<'a>) -> Self {
         Self {
             string,
-            options: StringOptions::default(),
+            quoting: Quoting::default(),
         }
     }
 
-    pub(crate) fn with_options(mut self, options: StringOptions) -> Self {
-        self.options = options;
+    pub(crate) fn with_options(mut self, options: Quoting) -> Self {
+        self.quoting = options;
         self
     }
 }
@@ -223,11 +186,11 @@
 
         let mut joiner = f.join_with(in_parentheses_only_soft_line_break_or_space());
 
-        for part in self.string.parts() {
+        for part in self.string.parts(self.quoting) {
             joiner.entry(&format_args![
                 line_suffix_boundary(),
                 leading_comments(comments.leading(&part)),
-                FormatStringPart::new(&part, self.options),
+                part,
                 trailing_comments(comments.trailing(&part))
             ]);
         }
```

I recommend taking a second look where omitting a call to `with_options` (and assuming the default) leads to invalid results and consider removing the `AsFormat` and `IntoFormat` implementations for these types (to force callers to specify all fields when cosntructing the `Format*`. 

> **Note**: It seems GitHub (reviewed with VS code) lost a few of my comments or even misplaced them. I'll try to identify the missing ones but the patch above should include all places where we can replace `StringOptions` with more specific types (to the point where we can delete `StringOptions` all together)

---

_@MichaReiser reviewed on 2023-12-14 02:32_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_string_literal.rs`:16 on 2023-12-14 02:32_

It seems the comment here went lost for some reason. 

We could avoid using the generic `StringOptions` here by introducing an `ExprStringLiteralKind` that is either `String` or `Docstring` and do the same for `StringLiteral` but with an extra `InFString` variant. It may help future readers to better understand which invariants are possible (and from where the formattign is called). 

```rust
#[derive(Default)]
pub struct FormatExprStringLiteral {
    options: ExprStringLiteralKind,
}

#[derive(Default, Copy, Clone, Debug)]
pub enum ExprStringLiteralKind {
    #[default]
    String,
    Docstring,
}

impl ExprStringLiteralKind {
    const fn string_literal_kind(self) -> StringLiteralKind {
        match self {
            ExprStringLiteralKind::String => StringLiteralKind::String,
            ExprStringLiteralKind::Docstring => StringLiteralKind::Docstring,
        }
    }

    const fn is_docstring(self) -> bool {
        matches!(self, Self::Docstring)
    }
}


#[derive(Default)]
pub struct FormatStringLiteral {
    layout: StringLiteralKind,
}

#[derive(Copy, Clone, Debug, Default)]
pub enum StringLiteralKind {
    #[default]
    String,
    Docstring,
    InFString(Quoting),
}
```

---

_@dhruvmanila reviewed on 2023-12-14 17:02_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/expression/binary_like.rs`:401 on 2023-12-14 17:02_

This is actually a good point. I missed this, and it is indeed required to detect the quoting for f-string and pass it along. I'll probably have to revert this change and move the quoting detection back to `AnyString` or find a different code path.

---

_@dhruvmanila reviewed on 2023-12-14 18:30_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/expression/binary_like.rs`:401 on 2023-12-14 18:30_

This wasn't too much to change. I added the `quoting` method back to `AnyString` which for an f-string will use the function `f_string_quoting` which is being used in 1 other place as well (`ExprFString`). This also means that `FormatStringContinuation` wouldn't accept `Quoting` as an argument but rather determine it on it's own and that's valid as it's a standalone expression. This keeps the simplification of binary like formatting for strings intact.

---

_Comment by @dhruvmanila on 2023-12-14 18:38_

Alright, this is looking good. I've made the suggested changes and some more:
1. Remove `StringOptions` and have specific options which are as suggested.
2. Custom implementation i.e., no `AsFormat` / `IntoFormat` for `FStringPart`, `FString`, and `StringLiteral` as the options for these nodes needs to be passed for every invocation (`with_options` needs to be called everytime).
3. `FormatStringContinuation` doesn't require the `Quoting` argument as it'll detect it through `AnyString::quoting`. This is because it's being called from binary like formatting and from each string expressions.

I'll merge this but feel free to add any further comments and I can take it up in a follow-up PR.

---

_@dhruvmanila reviewed on 2023-12-14 18:38_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/string/mod.rs`:224 on 2023-12-14 18:38_

This is resolved because `FormatStringContinuation` determines the quoting by itself through `AnyString::quoting`.

---

_@dhruvmanila reviewed on 2023-12-14 18:39_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/other/f_string_part.rs`:23 on 2023-12-14 18:39_

This is resolved by updating as per recommendation.

---

_@dhruvmanila reviewed on 2023-12-14 18:39_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/other/string_literal.rs`:11 on 2023-12-14 18:39_

We've removed the `StringContext` and introduced narrow types for better understanding and safety.

---

_Merged by @dhruvmanila on 2023-12-14 18:55_

---

_Closed by @dhruvmanila on 2023-12-14 18:55_

---

_Branch deleted on 2023-12-14 18:55_

---
