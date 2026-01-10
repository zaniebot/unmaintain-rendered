```yaml
number: 15818
title: Preserve triple quotes and prefixes for strings
type: pull_request
state: merged
author: ntBre
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: brent/preserve-triple
created_at: 2025-01-29T21:57:29Z
updated_at: 2025-02-04T13:41:08Z
url: https://github.com/astral-sh/ruff/pull/15818
synced_at: 2026-01-10T19:57:22Z
```

# Preserve triple quotes and prefixes for strings

---

_Pull request opened by @ntBre on 2025-01-29 21:57_

## Summary

This is a follow-up to #15726, #15778, and #15794 to preserve the triple quote and prefix flags in plain strings, bytestrings, and f-strings.

I also added a `StringLiteralFlags::without_triple_quotes` method to avoid passing along triple quotes in rules like SIM905 where it might not make sense, as discussed [here](https://github.com/astral-sh/ruff/pull/15726#discussion_r1930532426).

## Test Plan

Existing tests, plus many new cases in the `generator::tests::quote` test that should cover all combinations of quotes and prefixes, at least for simple string bodies.

Closes #7799 when combined with #15694, #15726, #15778, and #15794.

---

_Review requested from @carljm by @ntBre on 2025-01-29 21:57_

---

_Review requested from @MichaReiser by @ntBre on 2025-01-29 21:57_

---

_Review requested from @AlexWaygood by @ntBre on 2025-01-29 21:57_

---

_Review requested from @sharkdp by @ntBre on 2025-01-29 21:57_

---

_Review comment by @ntBre on `crates/ruff_python_codegen/src/generator.rs`:1763 on 2025-01-29 21:58_

I wrote these before the loop below. I think they are redundant with the loop version and could be deleted.

---

_@ntBre reviewed on 2025-01-29 21:58_

---

_@ntBre reviewed on 2025-01-29 22:02_

---

_Review comment by @ntBre on `crates/ruff_python_literal/src/escape.rs`:83 on 2025-01-29 22:02_

I think it might be worth defining a `Quote::as_triple_str` method and rewriting this as

```rust
let quote = if self.triple_quote {
    self.escape.layout().quote.as_triple_str()
} else {
    self.escape.layout().quote.as_str()
};
formatter.write_str(quote)?;
```

but I just went with this simple/dumb approach first.

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1489 on 2025-01-29 22:02_

could we add this method to `BytesLiteralFlags`, `FStringFlags` and `AnyStringFlags` as well? We might not need it now on those structs, but it's nice to have a consistent interface between the various structs

---

_Comment by @github-actions[bot] on 2025-01-29 22:07_

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

_Review comment by @AlexWaygood on `crates/ruff_python_codegen/src/generator.rs`:175 on 2025-01-29 22:07_

```suggestion
        self.p(&flags.format_string_contents(body));
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_codegen/src/generator.rs`:185 on 2025-01-29 22:10_

Strictly speaking, this might be slightly less performant, since `format_string_contents` allocates a new `String`. I think it's worth it to avoid repetition, however. The `Generator` also shouldn't be _terribly_ performance-sensitive, since we only create autofixes if we found an error in the user's code that leads us to emit a diagnostic, _and_ we deem that the diagnostic is autofixable.

```suggestion
            self.p(&flags.format_string_contents(s));
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_codegen/src/generator.rs`:196 on 2025-01-29 22:11_

```suggestion
            .expect("Writing to a String buffer should never fail");
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_codegen/src/generator.rs`:161 on 2025-01-29 22:11_

```suggestion
            .expect("Writing to a String buffer should never fail");
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_codegen/src/generator.rs`:1767 on 2025-01-29 22:16_

you could consider using https://docs.rs/test-case/latest/test_case/ for these -- it would mean that each example is run as a separate subtest, so if any one of them fails the other ones will still be run as separate tests. We use it for tests elsewhere in the repo, e.g. https://github.com/astral-sh/ruff/blob/15d886a50213dcd4013db028eeef15a60d182f98/crates/red_knot_python_semantic/src/types.rs#L4721-L4735

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/display.rs`:101 on 2025-01-29 22:20_

Because Rust doesn't support keyword arguments, it can be a little unreadable to have functions that take boolean parameters. At a callsite like here, it can be hard to tell why a `bool` is being passed into a function. It's often nicer to create a custom enum that can be passed into the function, e.g.

```rs
#[derive(Debug, Copy, Clone, PartialEq, Eq)]
enum TripleQuoted {
    Yes,
    No,
}
```

And then if the parameter took an instance of this enum, this callsite could be

```suggestion
                escape.bytes_repr(TripleQuoted::No).write(f)
```

---

_@AlexWaygood reviewed on 2025-01-29 22:23_

Nice!

---

_@ntBre reviewed on 2025-01-29 22:26_

---

_Review comment by @ntBre on `crates/ruff_python_codegen/src/generator.rs`:185 on 2025-01-29 22:26_

Wow thank you! I must have scrolled past this method so many times in the past few days!

---

_@AlexWaygood reviewed on 2025-01-29 22:28_

---

_Review comment by @AlexWaygood on `crates/ruff_python_codegen/src/generator.rs`:185 on 2025-01-29 22:28_

Hehe, it's on the `StringFlags` trait that `StringLiteralFlags`, `BytesLiteralFlags`, `FStringFlags` and `AnyStringFlags` all implement :-)

---

_Label `bug` added by @AlexWaygood on 2025-01-29 22:32_

---

_Label `fixes` added by @AlexWaygood on 2025-01-29 22:32_

---

_@ntBre reviewed on 2025-01-29 22:40_

---

_Review comment by @ntBre on `crates/ruff_python_codegen/src/generator.rs`:175 on 2025-01-29 22:40_

What did you think about the `str::from_utf8` call above this? As far as I know this should be a safe `unwrap` or `unreachable!`, but I went with this approach just in case. I figured it was better to fall back on the normal escaping code than to panic if I were wrong, but it does complicate the flow a bit.

---

_@ntBre reviewed on 2025-01-29 23:30_

---

_Review comment by @ntBre on `crates/ruff_python_codegen/src/generator.rs`:1767 on 2025-01-29 23:30_

Thanks for the suggestion! I thought the [test_matrix](https://docs.rs/test-case/latest/test_case/attr.test_matrix.html) macro looked perfect for this, but it appears to lowercase/further normalize the arguments, which causes a huge number of name conflicts for usage like this:

```rust
    /// test all of the valid string literal prefix and quote combinations from
    /// https://docs.python.org/3/reference/lexical_analysis.html#string-and-bytes-literals
    #[test_case::test_matrix(
        [
            ("r", "r"),
            ("u", "u"),
            ("R", "R"),
            ("U", "u"), // case not tracked
            ("f", "f"),
            ("F", "f"),   // f case not tracked
            ("fr", "rf"), // r before f
            ("Fr", "rf"), // f case not tracked, r before f
            ("fR", "Rf"), // r before f
            ("FR", "Rf"), // f case not tracked, r before f
            ("rf", "rf"),
            ("rF", "rf"), // f case not tracked
            ("Rf", "Rf"),
            ("RF", "Rf"), // f case not tracked
            // bytestrings
            ("b", "b"),
            ("B", "b"),   // b case
            ("br", "rb"), // r before b
            ("Br", "rb"), // b case, r before b
            ("bR", "Rb"), // r before b
            ("BR", "Rb"), // b case, r before b
            ("rb", "rb"),
            ("rB", "rb"), // b case
            ("Rb", "Rb"),
            ("RB", "Rb"), // b case
        ],
        ["\"", "'", "\"\"\"", "'''"],
        ["hello", "{hello} {world}"]
    )]
    fn prefix_quotes((inp, out): (&str, &str), quote: &str, base: &str) {
        let input = format!("{inp}{quote}{base}{quote}");
        let output = format!("{out}{quote}{base}{quote}");
        assert_eq!(round_trip(&input), output);
    }
```

I've tried a few things to get around it like giving the tuples unused numeric id fields, but no luck yet. I might need to do that for both the tuples *and* the quote fields.

I think this might even be an issue for [their docs](https://github.com/frondeus/test-case/issues/144).

---

_@ntBre reviewed on 2025-01-29 23:33_

---

_Review comment by @ntBre on `crates/ruff_python_codegen/src/generator.rs`:1767 on 2025-01-29 23:33_

Yeah okay, `u8`s on the end of those two fields did the job!

---

_Review comment by @AlexWaygood on `crates/ruff_python_codegen/src/generator.rs`:1763 on 2025-01-30 13:37_

Yeah, it makes sense to get rid of them, I think

---

_Review comment by @AlexWaygood on `crates/ruff_python_codegen/src/generator.rs`:1737 on 2025-01-30 13:38_

I nearly suggested a silly change to `BytesRepr` and `StrRepr` that would have broken some edge cases. Luckily some tests failed -- but only for the `StrRepr` change; no tests failed for the `BytesRepr` change. Here's a test that would have failed on my bad patch, which might be good to add:

```suggestion
        assert_eq!(round_trip(r#""he\"llo""#), r#"'he"llo'"#);
        assert_eq!(round_trip(r#"b"he\"llo""#), r#"b'he"llo'"#);
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_literal/src/escape.rs`:34 on 2025-01-30 13:42_

nit

```suggestion
    pub const fn is_yes(&self) -> bool {
```

---

_@AlexWaygood reviewed on 2025-01-30 13:43_

---

_@dhruvmanila reviewed on 2025-01-30 14:58_

---

_Review comment by @dhruvmanila on `crates/ruff_python_literal/src/escape.rs`:426 on 2025-01-30 14:58_

nit: is it worth adding a `write_n_char(n: usize, ch: char)` method?

---

_@ntBre reviewed on 2025-01-30 15:30_

---

_Review comment by @ntBre on `crates/ruff_python_literal/src/escape.rs`:426 on 2025-01-30 15:30_

I just merged a commit from Alex reworking the triple quote API that I think addresses this, but I like the `write_n_char` idea too!

---

_Review comment by @AlexWaygood on `crates/ruff_python_codegen/src/generator.rs`:172 on 2025-01-30 16:29_

I guess we could add a `StringFlags::write_string_contents()` method to avoid the allocation involved in `StringFlags::format_string_contents()`:

```diff
diff --git a/crates/ruff_python_ast/src/nodes.rs b/crates/ruff_python_ast/src/nodes.rs
index db45b5b3d..663299c5a 100644
--- a/crates/ruff_python_ast/src/nodes.rs
+++ b/crates/ruff_python_ast/src/nodes.rs
@@ -1033,6 +1033,14 @@ pub trait StringFlags: Copy {
         let quote_str = self.quote_str();
         format!("{prefix}{quote_str}{contents}{quote_str}")
     }
+
+    fn write_string_contents(self, buffer: &mut String, contents: &str) {
+        let quote_str = self.quote_str();
+        buffer.push_str(self.prefix().as_str());
+        buffer.push_str(quote_str);
+        buffer.push_str(contents);
+        buffer.push_str(quote_str);
+    }
 }
 
 bitflags! {
diff --git a/crates/ruff_python_codegen/src/generator.rs b/crates/ruff_python_codegen/src/generator.rs
index 78642ec7d..4727e9009 100644
--- a/crates/ruff_python_codegen/src/generator.rs
+++ b/crates/ruff_python_codegen/src/generator.rs
@@ -168,15 +168,14 @@ impl<'a> Generator<'a> {
         s: &[u8],
         flags: BytesLiteralFlags,
     ) -> Result<(), std::str::Utf8Error> {
-        let body = std::str::from_utf8(s)?;
-        self.p(&flags.format_string_contents(body));
+        flags.write_string_contents(&mut self.buffer, std::str::from_utf8(s)?);
         Ok(())
     }
 
     fn p_str_repr(&mut self, s: &str, flags: impl Into<AnyStringFlags>) {
         let flags = flags.into();
         if flags.prefix().is_raw() {
-            self.p(&flags.format_string_contents(s));
+            flags.write_string_contents(&mut self.buffer, s);
             return;
         }
```

It would be similar to the `write_*` methods in https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_literal/src/escape.rs -- except I'm not sure it needs to take any `impl Write` object; if we say it only takes an `&mut String` then it doesn't need to return a `Result`.

---

_Review comment by @AlexWaygood on `crates/ruff_python_literal/src/escape.rs`:70 on 2025-01-30 16:29_

```suggestion
    pub fn str_repr<'r>(&'a self, triple_quotes: TripleQuotes) -> StrRepr<'r, 'a> {
        StrRepr {
            escape: self,
            triple_quotes,
        }
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_literal/src/escape.rs`:76 on 2025-01-30 16:29_

```suggestion
    triple_quotes: TripleQuotes,
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_literal/src/escape.rs`:83 on 2025-01-30 16:30_

```suggestion
            .with_triple_quotes_set_to(self.triple_quotes);
```

---

_@AlexWaygood reviewed on 2025-01-30 16:30_

---

_@ntBre reviewed on 2025-01-30 17:00_

---

_Review comment by @ntBre on `crates/ruff_python_codegen/src/generator.rs`:172 on 2025-01-30 17:00_

This makes sense. Is it okay to skip the `self.num_newlines` checks in `Generator::p`? I've tended toward using that instead of writing directly to the buffer just in case, but I'm not sure exactly when it's relevant.

---

_@AlexWaygood reviewed on 2025-01-30 17:07_

---

_Review comment by @AlexWaygood on `crates/ruff_python_codegen/src/generator.rs`:172 on 2025-01-30 17:07_

Uhh... IDK really. We seem to already write to the buffer directly on `main` in some places?

https://github.com/astral-sh/ruff/blob/b0b8b062410afdee0c9948a23fc99de8e5a406b6/crates/ruff_python_codegen/src/generator.rs#L154

https://github.com/astral-sh/ruff/blob/b0b8b062410afdee0c9948a23fc99de8e5a406b6/crates/ruff_python_codegen/src/generator.rs#L162

---

_Review comment by @ntBre on `crates/ruff_python_codegen/src/generator.rs`:172 on 2025-01-30 17:17_

Oh right, at least we'll be in good company!

---

_@ntBre reviewed on 2025-01-30 17:17_

---

_@AlexWaygood reviewed on 2025-01-30 17:17_

---

_Review comment by @AlexWaygood on `crates/ruff_python_codegen/src/generator.rs`:175 on 2025-01-30 17:17_

Right now it doesn't seem like it's possible to construct an AST node for a literal-bytes expression that includes a non-ASCII character in it: https://play.ruff.rs/0b3b0efb-74cd-4e26-a885-9180e23e4506

However, our parser supports error recovery and the specifics of exactly what nodes representing invalid syntax will look like are an implementation detail. Similarly, we don't currently run any of our AST-based rules on files that contain invalid syntax (let alone try to autofix them), but that might well change in the future.

All told, I _think_ I prefer your current approach here, though I'd also be interested in @dhruvmanila's thoughts. It might be good to add a comment, whatever we end up doing here!

---

_Review comment by @ntBre on `crates/ruff_python_codegen/src/generator.rs`:175 on 2025-01-30 18:40_

That makes sense, thanks! I expanded my comment in `p_bytes_repr` a bit more, hopefully capturing some of this information. Otherwise I'm interested in Dhruv's thoughts too!

This seems a bit separate, but I guess we could even make a stronger assertion that `s.iter().all(u8::is_ascii)` if we wanted. Although we'd still need to call `from_utf8` to get something to write in our buffer anyway.

---

_@ntBre reviewed on 2025-01-30 18:40_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1043 on 2025-01-30 18:45_

oh, and I guess this is now a strict generalisation of the `format_string_contents()` method, so we can make now this simplification 😄

```diff
     fn format_string_contents(self, contents: &str) -> String {
-        let prefix = self.prefix();
-        let quote_str = self.quote_str();
-        format!("{prefix}{quote_str}{contents}{quote_str}")
+        let mut buffer = String::new();
+        self.write_string_contents(&mut buffer, contents);
+        buffer
     }
```

---

_@AlexWaygood approved on 2025-01-30 18:48_

Great work! The only other thing I'd do is get rid of the `Default` implementation for `AnyStringFlags`, for a consistent interface to the other `*Flags` structs

---

_Comment by @ntBre on 2025-01-30 19:36_

Thank you! Sorry I neglected the `Default` change from our conversation earlier. It looks like `AnyStringFlags::default` is only used in `AnyStringFlags::new`. I added a private `empty` method instead for now, but I could also make `empty` public, and add all of the docs from the other `Flags`. What do you think?

---

_Comment by @AlexWaygood on 2025-01-30 19:40_

> I added a private `empty` method instead for now

That works for me! Or we could just inline it into `AnyStringFlags::new()`

---

_@ntBre reviewed on 2025-01-30 20:23_

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/nodes.rs`:1043 on 2025-01-30 20:23_

Nice! I also added an initial size for the buffer.

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:20 on 2025-01-30 21:04_

```suggestion
use crate::{
    int,
    str::{Quote, TripleQuotes},
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1033 on 2025-01-30 21:05_

or maybe we could move this optimization to `write_string_contents()`?

```diff
diff --git a/crates/ruff_python_ast/src/nodes.rs b/crates/ruff_python_ast/src/nodes.rs
index deb25ef05..da0bc5857 100644
--- a/crates/ruff_python_ast/src/nodes.rs
+++ b/crates/ruff_python_ast/src/nodes.rs
@@ -1029,13 +1029,14 @@ pub trait StringFlags: Copy {
     }
 
     fn format_string_contents(self, contents: &str) -> String {
-        let buf_size = self.opener_len().to_usize() + contents.len() + self.closer_len().to_usize();
-        let mut buffer = String::with_capacity(buf_size);
+        let mut buffer = String::new();
         self.write_string_contents(&mut buffer, contents);
         buffer
     }
 
     fn write_string_contents(self, buffer: &mut String, contents: &str) {
+        buffer
+            .reserve(self.opener_len().to_usize() + contents.len() + self.closer_len().to_usize());
         let quote_str = self.quote_str();
         buffer.push_str(self.prefix().as_str());
         buffer.push_str(quote_str);
```

---

_@AlexWaygood approved on 2025-01-30 21:07_

---

_@AlexWaygood approved on 2025-01-30 23:39_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:2148 on 2025-01-31 05:41_

I'd suggest to keep the name as `with_triple_quotes` as "with" and "set_to" sounds similar and similar to getter conventions, we wouldn't usually include the "set" prefix unless it's obvious what's being set (https://rust-lang.github.io/api-guidelines/naming.html#getter-names-follow-rust-convention-c-getter).

---

_@dhruvmanila reviewed on 2025-01-31 05:41_

---

_@AlexWaygood reviewed on 2025-01-31 11:31_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:2148 on 2025-01-31 11:31_

That's fair. I suggested the name `with_triple_quotes_set_to` as I thought `flags.with_triple_quotes(TripleQuotes::No)` read a bit weird. The name `with_triple_quotes` implies to me that calling the method will always return a version of `flags` with the `TRIPLE_QUOTED` flag set. But in fact if you pass in `TripleQuotes::No`, this call returns a version of `flags` with the `TRIPLE_QUOTED` flag _unset_. Maybe `with_triple_quotes_set_to` just ends up adding verbosity without adding any more clarity, though.

TL;DR: I'm fine with changing it back to `with_triple_quotes`, since I can't think of anything obviously better 😄

---

_@ntBre reviewed on 2025-01-31 13:04_

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/nodes.rs`:2148 on 2025-01-31 13:04_

I think I lean toward changing it back too. With no arguments I'd definitely expect `with_triple_quotes` to set the flag (as it did before), but the enum argument helps to clarify what's going on now.

---

_@dhruvmanila reviewed on 2025-02-01 05:54_

---

_Review comment by @dhruvmanila on `crates/ruff_python_codegen/src/generator.rs`:175 on 2025-02-01 05:54_

Sorry for the late reply. The main goal of error recovery would be to preserve all the information of the original source i.e., the parser shouldn't drop any of the tokens. This would happen on a case by case basis but, for example, in this scenario the parser could just store the raw bytes and set a flag that it contains non-ascii bytes. All downstream tools would then need to account for this additional information.

Another thing to point out is that the generator is mainly used to provide auto-fix and we might decide that even though we would provide diagnostics for source code with syntax errors, we won't be providing any auto-fix for it as it might prove even more difficult to consider all the cases.

I think the current logic is fine for now and we can think about this more when we start working on this.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1044 on 2025-02-03 09:08_

Nit: I'm leaning towards a `display` method here. You then get the: *Write into a new string* (`flags.display(contents).to_string()`) or *write to an existing buffer* (`write!(&mut s, "{}", flags.display(contents))`) for free.

```rust

trait StringFlags {
	...

	fn display_contents(contents: &str) -> DisplayFlags {
		DisplayFlags { prefix: self.prefix(), quote_str: self.quote_str, contents }
	}
}

pub struct DisplayFlags<'a>  {
	prefix: AnyStringPrefix,
	quote_str: &'a str,
	contents: &'a str,
}

impl fmt::Dispaly for DisplayFlags<'_> { ..}

```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_simplify/rules/split_static_string.rs`:136 on 2025-02-03 09:10_

Does the generator take care of escaping nested quotes? E.g. if you have 

```python
"""
"itemA"
'itemB'
'''itemC'''
""".split()
```

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/generator.rs`:174 on 2025-02-03 09:18_

Nit: I'm leaning towards inlining this method as it is only used in one place and not having to jump to `p_raw_bytes` when reading the comment in `p_bytes_repr` could help readability

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/generator.rs`:155 on 2025-02-03 09:20_

Does this lead to a syntax error if `flags` uses a quote that is contained in the string? For example, what if I have `b" and 'tert"` and flags specify single quotes?

---

_@MichaReiser reviewed on 2025-02-03 09:23_

I've mainly an understand question: 

Who's responsibility is it to correctly handle nested quotes? Is it the generator or the code building the AST? If it is the generator, what happens in cases where it's impossible to pick the right quotes?

---

_@ntBre reviewed on 2025-02-03 14:28_

---

_Review comment by @ntBre on `crates/ruff_python_codegen/src/generator.rs`:155 on 2025-02-03 14:28_

I think that is true, but the only way to get into that state is if the user of the `Generator` hard-coded a request for single quotes. The intention was to pass along flags from input string literals (so the input would have to be `br' and 'tert'`, which is already a syntax error, or to use [`Checker::default_bytes_flags`](https://github.com/astral-sh/ruff/blob/brent%2Fpreserve-triple/crates/ruff_linter/src/checkers/ast/mod.rs#L314) if there's not an existing literal to preserve, which I  think should also handle this case.

Basically you'd have to write code like
```rust
BytesLiteral {
            value: Box::from(*b" and 'tert"),
            range: TextRange::default(),
            flags: BytesLiteralFlags::empty()
                .with_quote_style(Quote::Single)
                .with_prefix(ByteStringPrefix::Raw { uppercase_r: false }),
        }
```

to cause the issue, at least as far as I can think of.

---

_Comment by @ntBre on 2025-02-03 14:29_

> Who's responsibility is it to correctly handle nested quotes? Is it the generator or the code building the AST? If it is the generator, what happens in cases where it's impossible to pick the right quotes?

It's still the generator's responsibility to handle nested quotes. All of the code paths except raw strings end up going through either [`UnicodeEscape::with_preferred_quote`](https://github.com/astral-sh/ruff/blob/brent%2Fpreserve-triple/crates/ruff_python_literal/src/escape.rs#L57) (in the case of normal strings and f-strings) or [`AsciiEscape::with_preferred_quote`](https://github.com/astral-sh/ruff/blob/brent%2Fpreserve-triple/crates/ruff_python_literal/src/escape.rs#L250) (for bytestrings). These try to use the quote you pass in but will change the outer quotes if needed or even start adding escaped quotes in the worst case.

I tested your `split` case above with SIM905 (after temporarily modifying the rule to preserve triple quotes) and the output was `['''"itemA"''', """'itemB'""", """'''itemC'''"""]`. `itemA` is probably the most interesting because the outer triple double quotes from the input got converted to single quotes to preserve the inner double quotes.

---

_@MichaReiser reviewed on 2025-02-03 14:53_

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/generator.rs`:155 on 2025-02-03 14:53_

The exception to this would be if a refactor e.g moves a byte string into an f-string expression and the target python version is 3.12. But arguably, there isn't much the generator can do about this.

---

_@MichaReiser approved on 2025-02-03 14:53_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/rules/split_static_string.rs`:136 on 2025-02-03 15:29_

Added this as a SIM905 test case, including `"'itemD'"` to show escaped quotes when the generator can't just alternate.

Input:

```python
"""
"itemA"
'itemB'
'''itemC'''
"'itemD'"
""".split()
```

Output:

```python
['"itemA"', "'itemB'", "'''itemC'''", "\"'itemD'\""]
```

---

_@ntBre reviewed on 2025-02-03 15:29_

---

_Review comment by @ntBre on `crates/ruff_python_codegen/src/generator.rs`:155 on 2025-02-03 17:21_

Oh good point, I forgot about rules moving these into f-strings. I think `UnicodeEscape` will still get a chance to run in that case, as long as the outer f-string isn't raw too, but this is good to keep in mind.

---

_@ntBre reviewed on 2025-02-03 17:21_

---

_@AlexWaygood reviewed on 2025-02-03 17:36_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_simplify/SIM905.py`:17 on 2025-02-03 17:36_

nit: this unfortunately results in a huge snapshot change that's quite hard to review, because the line numbers change for all diagnostics emitted on code below the added lines here. I know it means it doesn't "fit in" with the "categories" in the fixture that the comments crate, but I think I'd prefer for this to be added at the bottom of the fixture file, if that's okay?

---

_@ntBre reviewed on 2025-02-03 17:38_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_simplify/SIM905.py`:17 on 2025-02-03 17:38_

For sure, I prefer it at the bottom too but tried to stick with the categories. I'll just add a comment to clarify.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_simplify/SIM905.py`:17 on 2025-02-03 17:47_

I didn't rebase over the last one, but hopefully the net diff is much more readable now!

---

_@ntBre reviewed on 2025-02-03 17:47_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1055 on 2025-02-03 17:52_

An alternative here might be this (the diff is relative to your branch), which would mean the `DisplayFlags` struct would take up slightly less memory. Curious what @MichaReiser thinks of the pros and cons. I think there's almost certainly not much practical difference to what you have now:

```diff
diff --git a/crates/ruff_python_ast/src/nodes.rs b/crates/ruff_python_ast/src/nodes.rs
index 3471776f2..70b21e7db 100644
--- a/crates/ruff_python_ast/src/nodes.rs
+++ b/crates/ruff_python_ast/src/nodes.rs
@@ -977,7 +977,7 @@ impl Ranged for FStringPart {
     }
 }
 
-pub trait StringFlags: Copy {
+pub trait StringFlags: Copy + Into<AnyStringFlags> {
     /// Does the string use single or double quotes in its opener and closer?
     fn quote_style(self) -> Quote;
 
@@ -1029,16 +1029,14 @@ pub trait StringFlags: Copy {
 
     fn display_contents(self, contents: &str) -> DisplayFlags {
         DisplayFlags {
-            prefix: self.prefix(),
-            quote_str: self.quote_str(),
+            flags: self.into(),
             contents,
         }
     }
 }
 
 pub struct DisplayFlags<'a> {
-    prefix: AnyStringPrefix,
-    quote_str: &'a str,
+    flags: AnyStringFlags,
     contents: &'a str,
 }
 
@@ -1047,8 +1045,8 @@ impl std::fmt::Display for DisplayFlags<'_> {
         write!(
             f,
             "{prefix}{quote}{contents}{quote}",
-            prefix = self.prefix,
-            quote = self.quote_str,
+            prefix = self.flags.prefix(),
+            quote = self.flags.quote_str(),
             contents = self.contents
         )
     }
diff --git a/crates/ruff_python_parser/src/token.rs b/crates/ruff_python_parser/src/token.rs
index ecda19730..d741b15a4 100644
--- a/crates/ruff_python_parser/src/token.rs
+++ b/crates/ruff_python_parser/src/token.rs
@@ -777,6 +777,12 @@ impl TokenFlags {
     }
 }
 
+impl From<TokenFlags> for AnyStringFlags {
+    fn from(flags: TokenFlags) -> Self {
+        flags.as_any_string_flags()
+    }
+}
+
```

---

_@AlexWaygood approved on 2025-02-03 17:52_

LGTM!

---

_@AlexWaygood reviewed on 2025-02-03 17:59_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1039 on 2025-02-03 17:59_

I don't _love_ the name of this struct because it doesn't just display the flags -- it displays the string contents embedded in the prefix and quotes, which are supplied by the flags. `DisplayStringLiteralWithQuotesAndPrefix` is kinda verbose, though, so I'm not sure what a better name might be. (AKA: I'm fine with it being merged as-is!)

---

_@ntBre reviewed on 2025-02-03 18:04_

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/nodes.rs`:1039 on 2025-02-03 18:04_

Oh good point. I also wanted to make the struct private, but it's in a public trait. One thing we could do is just return `impl Display` from `display_contents`? Then we could use as verbose an internal name as we want and arguably better capture the intention here without having to name a type.

---

_@AlexWaygood reviewed on 2025-02-03 18:05_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1039 on 2025-02-03 18:05_

Oh, I like that idea!

---

_@MichaReiser reviewed on 2025-02-03 19:36_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1055 on 2025-02-03 19:36_

I'm not too fond of trait chains. I'd rather have a `into_any_string_flags` method but I don't think it matters much. `Display` is rarely used.

---

_@MichaReiser reviewed on 2025-02-03 19:37_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1039 on 2025-02-03 19:37_

The downside of that is that the trait is no longer object safe, but it might not matter here.

---

_@ntBre reviewed on 2025-02-03 19:56_

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/nodes.rs`:1039 on 2025-02-03 19:56_

Oh good point. I'm somewhat leaning toward leaving the code as it is here, for both the struct name and its contents (re the conversation above) if that's okay with you both.

I don't really love the new trait bound or `From` impl either, but it does make the struct smaller, so I'm happy either way, though.

---

_@AlexWaygood reviewed on 2025-02-03 20:52_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1039 on 2025-02-03 20:52_

I'm fine to leave everything as is!

---

_Merged by @ntBre on 2025-02-04 13:41_

---

_Closed by @ntBre on 2025-02-04 13:41_

---

_Branch deleted on 2025-02-04 13:41_

---
