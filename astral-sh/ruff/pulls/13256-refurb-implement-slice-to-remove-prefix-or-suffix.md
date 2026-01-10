```yaml
number: 13256
title: "[refurb] Implement `slice-to-remove-prefix-or-suffix` (`FURB188`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: remove-affix
created_at: 2024-09-05T17:51:05Z
updated_at: 2024-09-09T15:42:06Z
url: https://github.com/astral-sh/ruff/pull/13256
synced_at: 2026-01-10T21:38:32Z
```

# [refurb] Implement `slice-to-remove-prefix-or-suffix` (`FURB188`)

---

_Pull request opened by @dylwil3 on 2024-09-05 17:51_

This PR implements `FURB188` which suggests the use of `removeprefix`/`removesuffix` in order to remove prefixes and suffixes from strings, rather than slicing the string conditionally upon the output of `startswith`/`endswith`.

Tested using same test suite as `refurb`.

Moves the needle on #1348.


---

_Comment by @dylwil3 on 2024-09-05 17:52_

Not super happy with how gnarly the implementation is - so open to any feedback/tips to clean it up. I've tried to add comments wherever possible so hopefully it is at least readable.

Definitely found myself wishing that Rust supported matching patterns involving structs with heavily nested `Box`es... (https://github.com/rust-lang/rust/issues/87121)

---

_Comment by @github-actions[bot] on 2024-09-05 18:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+7 -0 violations, +0 -0 fixes in 3 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L511'>src/bokeh/util/compiler.py:511:9:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/plugins/file_server.py#L63'>tests/support/plugins/file_server.py:63:9:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/43d3e445fb4a4cf4010b74692df00ed267ec3e34/rotkehlchen/assets/converters.py#L57'>rotkehlchen/assets/converters.py:57:5:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
+ <a href='https://github.com/rotki/rotki/blob/43d3e445fb4a4cf4010b74692df00ed267ec3e34/rotkehlchen/exchanges/bitfinex.py#L575'>rotkehlchen/exchanges/bitfinex.py:575:9:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/20bbc71b63ff3a7dc72fab9a0b27ea81b7ec1be8/indico/cli/devserver.py#L220'>indico/cli/devserver.py:220:9:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
+ <a href='https://github.com/indico/indico/blob/20bbc71b63ff3a7dc72fab9a0b27ea81b7ec1be8/indico/core/db/sqlalchemy/colors.py#L31'>indico/core/db/sqlalchemy/colors.py:31:13:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
+ <a href='https://github.com/indico/indico/blob/20bbc71b63ff3a7dc72fab9a0b27ea81b7ec1be8/indico/modules/networks/fields.py#L44'>indico/modules/networks/fields.py:44:9:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB188 | 7 | 7 | 0 | 0 | 0 |

</p>
</details>




---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:290 on 2024-09-07 14:00_

A little less nested:

```suggestion
    if body_name != test_name || test_name != else_or_target_name {
        return None;
    }

    let (affix_kind, bound) = match func_name {
        "startswith" if slice.upper.is_none() => (AffixKind::StartsWith, slice.lower.as_ref()?),
        "endswith" if slice.lower.is_none() => (AffixKind::EndsWith, slice.upper.as_ref()?),
        _ => return None,
    };

    Some(RemoveAffixData {
        text: body_name,
        bound,
        affix_query: AffixQuery {
            kind: affix_kind,
            affix,
        },
    })
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:245 on 2024-09-07 14:07_

I wonder if we _need_ to exclude other valid assignment targets, like attribute expressions. We could possibly use our [`ComparableExpr`](https://github.com/astral-sh/ruff/blob/a7c936878db8d9906c6ca91c8a1301aee80ee78a/crates/ruff_python_ast/src/comparable.rs#L1-L16) struct to facilitate comparisons of other kinds of assignment targets. It shouldn't really matter what kind of assignment target it is, as long as it's the same in all three places -- i.e. this should be fine for us to flag and fix:

```py
import foo.bar

SUFFIX = "suffix"

x = foo.bar.baz[:len(SUFFIX)] if foo.bar.baz.endswith(SUFFIX) else foo.bar.baz
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:4 on 2024-09-07 14:10_

teeny tiny nit:

```suggestion
use ruff_python_ast as ast;
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:306 on 2024-09-07 14:12_

should this be a method on `RemoveAffixData`, if the function takes no other arguments?

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:328 on 2024-09-07 14:15_

Is truncating the number the correct thing to do here, if it's larger than `u32::MAX`? If `x` _happens_ by random chance to be exactly equal to `u32::MAX`, I think this equality comparison would then incorrectly evaluate to `true`. Maybe this would be better?

```suggestion
            // Only support prefix removal for size at most `u32::MAX`
            u32::try_from(string_val.len()).is_ok_and(|length| x == &ast::Int::from(length))
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:340 on 2024-09-07 14:17_

This won't work if somebody is annoying and does something like this:

```py
from builtins import len as builtins_len
```

You want to use something like `checker.semantic().match_builtin_expr(func, "len")`, which will recognise even `builtins_len` as a reference to the `builtins.len()` function

https://github.com/astral-sh/ruff/blob/a4ebe7d34407ea353762ebde1fef4a718edddb55/crates/ruff_python_semantic/src/model.rs#L308-L326

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:365 on 2024-09-07 14:19_

same as above:

```suggestion
                    // Only support prefix removal for size at most `u32::MAX`
                    u32::try_from(string_val.len()).is_ok_and(|length| x == &ast::Int::from(length))
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:421 on 2024-09-07 14:21_

I think you might be able to do this more simply (and probably more cheaply) using some of the techniques I showed you in https://github.com/astral-sh/ruff/pull/13275 (string formatting, using `checker.locator().slice(<expr_node>)` to get the node as it exists in the source)

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:498 on 2024-09-07 14:22_

nit: some docstrings for the fields on these structs would be nice ;)

---

_@AlexWaygood reviewed on 2024-09-07 14:22_

Nice! I can see that a lot of work went into this

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:158 on 2024-09-08 07:51_

Nit: We normally use `_impl` to signal that this is the main implementation. 

Or you use `affix_remove_data` to indicate that it applies to all nodes.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:198 on 2024-09-08 07:52_

Nit
```suggestion
    let [statement] = body.as_slice() else {
        return None;
    };
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:213 on 2024-09-08 07:53_

Nit: 

```suggestion
    let [target] = targets.as_slice() else {
        return None;
    };
    let else_or_target_name = &target.as_name_expr()?.id;
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:267 on 2024-09-08 07:53_

Nit
```suggestion
    let [affix] = func_args.as_slice() else {
        return None;
    }
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:327 on 2024-09-08 07:54_

```suggestion
            let length = u32::from(string_val.text_len())
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:340 on 2024-09-08 07:55_

Nit

```suggestion
                .is_some_and(|name| &name.id == "len")
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:384 on 2024-09-08 07:56_

```suggestion
                    .is_some_and(|name| &name.id == "len")
```

---

_@MichaReiser reviewed on 2024-09-08 07:56_

I only skimmed over the rust code and left a few NIT comments.

---

_Label `rule` added by @MichaReiser on 2024-09-08 07:57_

---

_Label `preview` added by @MichaReiser on 2024-09-08 07:57_

---

_@dylwil3 reviewed on 2024-09-09 04:56_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:290 on 2024-09-09 04:56_

Done (though it looks a little different because I implemented one of your other suggestions as well)

---

_@dylwil3 reviewed on 2024-09-09 04:57_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:245 on 2024-09-09 04:57_

Rule expanded! (there's a typo in your example though: the slice needs a `-` preceding `len`). Test fixture updated as well.

---

_@dylwil3 reviewed on 2024-09-09 04:58_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:4 on 2024-09-09 04:58_

Well if you leave it around that's how you get lice! Nit picked

---

_@dylwil3 reviewed on 2024-09-09 04:59_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:306 on 2024-09-09 04:59_

This now takes the `checker` as argument so that I can use the `builtins` suggestion you made below - so I've left this as a function on its own.

---

_@dylwil3 reviewed on 2024-09-09 04:59_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:328 on 2024-09-09 04:59_

Done.

---

_@dylwil3 reviewed on 2024-09-09 04:59_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:340 on 2024-09-09 04:59_

Nice! Done.

---

_@dylwil3 reviewed on 2024-09-09 04:59_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:365 on 2024-09-09 04:59_

Also done.

---

_@dylwil3 reviewed on 2024-09-09 05:00_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:421 on 2024-09-09 05:00_

Great idea! Implemented

---

_@dylwil3 reviewed on 2024-09-09 05:00_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:498 on 2024-09-09 05:00_

Minimal docstrings added

---

_@dylwil3 reviewed on 2024-09-09 05:00_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:158 on 2024-09-09 05:00_

Changed to `affix_removal_data`

---

_@dylwil3 reviewed on 2024-09-09 05:00_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:198 on 2024-09-09 05:00_

picked

---

_@dylwil3 reviewed on 2024-09-09 05:00_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:213 on 2024-09-09 05:00_

picked

---

_@dylwil3 reviewed on 2024-09-09 05:01_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:267 on 2024-09-09 05:01_

picked (but with `as_ref` not `as_slice` since that's a `Box<[T]>` sorta thing)

---

_@dylwil3 reviewed on 2024-09-09 05:03_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:327 on 2024-09-09 05:03_

I used @AlexWaygood 's suggestion here instead - somehow couldn't get the `TextLen` trait to speak to `&StringLiteralValue` but maybe I was doing something wrong...

---

_@dylwil3 reviewed on 2024-09-09 05:04_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:340 on 2024-09-09 05:04_

Used `match_builtin_expr` instead

---

_@dylwil3 reviewed on 2024-09-09 05:04_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:384 on 2024-09-09 05:04_

same as above

---

_Comment by @dylwil3 on 2024-09-09 05:06_

Wow @AlexWaygood and @MichaReiser  - what a fantastic and thorough code review! Thanks so much! Learning a lot from you

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:327 on 2024-09-09 10:42_

It would be great to include a specific test for a situation where a user does something like `from builtins import len as builtins_len`

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:369 on 2024-09-09 10:43_

```suggestion
                checker.semantic().match_builtin_expr(func, "len")
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:296 on 2024-09-09 10:46_

The `Checker` is something of a huge class with a huge amount of "power" behind it. In this function I _think_ we only need an immutable reference to the `SemanticModel`, so I think it would more clearly indicate the scope of the things this function needs to do if the signature were instead

```suggestion
fn affix_matches_slice_bound(data: &RemoveAffixData, checker: &SemanticModel) -> bool {
```

And then in the body of the function you'd do `semantic.match_builtin_expr(func, "len")` instead of `checker.semantic().match_builtin_expr(func, "len")`

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:295 on 2024-09-09 10:47_

```suggestion
/// This function verifies that `bound == -len(suffix)` in two cases:
///   - `suffix` is a string literal and `bound` is a number literal
///   - `suffix` is an expression and `bound` is
///     exactly `-len(suffix)` (as AST nodes, prior to evaluation.)
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:232 on 2024-09-09 10:48_

nit: indentation of 2 spaces for MarkDown bullet points, and keep the whole bullet indented by the same margin ;)

```suggestion
///   - `value` and `else_or_target_name`
///     are equal to a common name `text`
///   - `test` is of the form `text.startswith(prefix)`
///     or `text.endswith(suffix)`
///   - `slice` has no upper bound in the case of a prefix,
///     and no lower bound in the case of a suffix
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:10 on 2024-09-09 10:50_

```suggestion
/// the string to a slice after checking `.startswith()` or `.endswith()`, respectively.
```

---

_Assigned to @AlexWaygood by @MichaReiser on 2024-09-09 10:51_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:34 on 2024-09-09 10:51_

```suggestion
/// The methods [`str.removeprefix`] and [`str.removesuffix`],
/// introduced in Python 3.9, have the same behavior
/// and are more readable and efficient.
///
/// ## Example
/// ```python
/// filename[:-4] if filename.endswith(".txt") else filename
/// ```
///
/// ```python
/// if text.startswith("pre"):
///     text = text[3:]
/// ```
///
/// Use instead:
/// ```python
/// filename = filename.removesuffix(".txt")
/// ```
///
/// ```python
/// text = text.removeprefix("pre")
/// ```
///
/// [`str.removeprefix`]: https://docs.python.org/3/library/stdtypes.html#str.removeprefix
/// [`str.removesuffix`]: https://docs.python.org/3/library/stdtypes.html#str.removesuffix
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:59 on 2024-09-09 10:57_

It's a little more verbose, but I'd be tempted to use methods on `AffixKind` for this, e.g.

<details>

```diff
diff --git a/crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs b/crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs
index e68bc711c..a7f5585e5 100644
--- a/crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs
+++ b/crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs
@@ -53,15 +53,13 @@ impl AlwaysFixableViolation for SliceToRemovePrefixOrSuffix {
     }
 
     fn fix_title(&self) -> String {
-        let (to_replace, replacement) = match self.affix_kind {
-            AffixKind::StartsWith => ("`startswith`", "`removeprefix`"),
-            AffixKind::EndsWith => ("`endswith`", "`removesuffix`"),
-        };
+        let method_name = self.affix_kind.as_str();
+        let replacement = self.affix_kind.replacement();
         let context = match self.stmt_or_expression {
             StmtOrExpr::Statement => "assignment",
             StmtOrExpr::Expression => "ternary expression",
         };
-        format!("Use {replacement} instead of {context} conditional upon {to_replace}.")
+        format!("Use {replacement} instead of {context} conditional upon {method_name}.")
     }
 }
 
@@ -438,6 +436,22 @@ enum AffixKind {
     EndsWith,
 }
 
+impl AffixKind {
+    const fn as_str(self) -> &'static str {
+        match self {
+            Self::StartsWith => "startswith",
+            Self::EndsWith => "endswith",
+        }
+    }
+
+    const fn replacement(self) -> &'static str {
+        match self {
+            Self::StartsWith => "removeprefix",
+            Self::EndsWith => "removesuffix",
+        }
+    }
+}
+
```

</details>

It feels slightly more elegant to push this logic elsewhere, and it feels like something a variant of `AffixKind` would "naturally" know about itself 😄

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:446 on 2024-09-09 10:59_

```suggestion
    /// Node representing the prefix or suffix being passed to the string method.
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:462 on 2024-09-09 11:00_

```suggestion
#[derive(Debug)]
struct RemoveAffixData<'a> {
    /// Node representing the string whose prefix or suffix we want to remove
    text: &'a ast::Expr,
    /// Node representing the bound used to slice the string
    bound: &'a ast::Expr,
    /// Contains the prefix or suffix used in `text.startswith(prefix)` or `text.endswith(suffix)`
    affix_query: AffixQuery<'a>,
}
```

---

_@AlexWaygood reviewed on 2024-09-09 11:02_

Very nice indeed! Basically just nits remaining, though there is one other location where I think you should use `match_builtin_expr()` for correctness

---

_@dylwil3 reviewed on 2024-09-09 14:14_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:327 on 2024-09-09 14:14_

done!

---

_@dylwil3 reviewed on 2024-09-09 14:15_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:369 on 2024-09-09 14:15_

done (but just with `semantic` as per below)

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:296 on 2024-09-09 14:15_

done!

---

_@dylwil3 reviewed on 2024-09-09 14:15_

---

_@dylwil3 reviewed on 2024-09-09 14:15_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:59 on 2024-09-09 14:15_

done! I initially changed the `self` to `&self` to satisfy the [naming conventions](https://rust-lang.github.io/api-guidelines/naming.html), but I guess clippy insisted I pass `self` by value (as you suggest), and none of the conventions seem to involve "owned --> borrowed" ... so I kept the name as in your suggestion

---

_Comment by @dylwil3 on 2024-09-09 14:16_

Thanks again! I think I picked the remaining nits.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:330 on 2024-09-09 14:21_

`semantic.match_builtin_expr()` is unfortunately more expensive than what you had before (we need to resolve the qualified name, which involves several hashmap lookups and some complex AST matching), so we should probably switch the order of these conditions:

```suggestion
            arguments.len() == 1
                && semantic.match_builtin_expr(func, "len")
```

(I should have thought of this earlier, sorry!)

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:371 on 2024-09-09 14:22_

```suggestion
                arguments.len() == 1
                    && semantic.match_builtin_expr(func, "len")
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:403 on 2024-09-09 14:23_

We can do this slightly more simply now that you've added the `AffixKind::replacement()` method!

```suggestion
    let replacement = affix_query.kind.replacement();
    format!("{text_str} = {text_str}.{replacement}({affix_str})")
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:427 on 2024-09-09 14:25_

```suggestion
    let replacement = affix_query.kind.replacement();
    format!("{text_str}.{replacement}({affix_str})")
```

---

_@AlexWaygood approved on 2024-09-09 14:26_

Fab! Some _tiny_ last remaining comments:

---

_@AlexWaygood reviewed on 2024-09-09 14:29_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:59 on 2024-09-09 14:29_

Yeah, for an enum this tiny, copying the data can actually be more efficient than borrowing `self`, even though that's not good practice for perhaps 90% of methods in Rust :-)

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:56 on 2024-09-09 14:32_

```suggestion
        let replacement = self.affix_kind.replacement();
        format!("Prefer `{replacement}` over conditionally replacing with slice.")
```

---

_@AlexWaygood reviewed on 2024-09-09 14:32_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:319 on 2024-09-09 14:39_

I think this is what @MichaReiser was suggesting earlier (I checked locally and this version seems to compile! You need to add an import of the `ruff_text_size::TextLen` trait though)

```suggestion
        ) => num
            .as_int()
            .and_then(ast::Int::as_u32) // Only support prefix removal for size at most `u32::MAX`
            .is_some_and(|x| x == string_val.to_str().text_len().to_u32()),
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:353 on 2024-09-09 14:43_

```suggestion
                // Only support prefix removal for size at most `u32::MAX`
                value
                    .as_int()
                    .and_then(ast::Int::as_u32)
                    .is_some_and(|x| x == string_val.to_str().text_len().to_u32())
```

---

_@AlexWaygood reviewed on 2024-09-09 14:43_

---

_@dylwil3 reviewed on 2024-09-09 14:54_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:56 on 2024-09-09 14:54_

This one I kept as is, because of this suggestion from a previous PR: https://github.com/astral-sh/ruff/pull/12691#discussion_r1704822876

---

_@AlexWaygood reviewed on 2024-09-09 14:55_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:56 on 2024-09-09 14:55_

Oh nice, thank you! I didn't consider the docs-generation impact of this :-)

---

_@AlexWaygood approved on 2024-09-09 15:01_

Thanks!

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:401 on 2024-09-09 15:02_

```suggestion
    let affix_str = locator.slice(affix_query.affix);
    let replacement = affix_query.kind.replacement();
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:423 on 2024-09-09 15:02_

```suggestion
    let affix_str = locator.slice(affix_query.affix);
    let replacement = affix_query.kind.replacement();
```

---

_@AlexWaygood reviewed on 2024-09-09 15:02_

---

_Merged by @AlexWaygood on 2024-09-09 15:08_

---

_Closed by @AlexWaygood on 2024-09-09 15:08_

---

_Branch deleted on 2024-09-09 15:42_

---
