```yaml
number: 13636
title: "[red-knot] type inference/checking test framework"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/test-framework
created_at: 2024-10-05T02:50:29Z
updated_at: 2024-10-08T19:33:21Z
url: https://github.com/astral-sh/ruff/pull/13636
synced_at: 2026-01-10T20:59:36Z
```

# [red-knot] type inference/checking test framework

---

_Pull request opened by @carljm on 2024-10-05 02:50_

## Summary

Adds a markdown-based test framework for writing tests of type inference and type checking. Fixes #11664.

Implements the basic required features. A markdown test file is a suite of tests, each test can contain one or more Python files, with optionally specified path/name. The test writes all files to an in-memory file system, runs red-knot, and matches the resulting diagnostics against `Type: ` and `Error: ` assertions embedded in the Python source as comments.

We will want to add features like incremental tests, setting custom configuration for tests, writing non-Python files, testing syntax errors, capturing full diagnostic output, etc. There's also plenty of room for improved UX (colored output?).

This is a large PR, but I split the commits carefully for easier review. I'm preferring this over stacked PRs, since the GH UX for stacked PRs is so bad. I encourage reviewers to use the GH feature to narrow review per-commit, for less daunting review.

## Test Plan

Lots of tests!

Sample of the current output when a test fails:

```
     Running tests/inference.rs (target/debug/deps/inference-7c96590aa84de2a4)

running 1 test
test inference::path_1_resources_inference_numbers_md ... FAILED

failures:

---- inference::path_1_resources_inference_numbers_md stdout ----
inference/numbers.md - Numbers - Floats
  /src/test.py
    line 2: unexpected error: [invalid-assignment] "Object of type `Literal["str"]` is not assignable to `int`"

thread 'inference::path_1_resources_inference_numbers_md' panicked at crates/red_knot_test/src/lib.rs:60:5:
Some tests failed.
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace


failures:
    inference::path_1_resources_inference_numbers_md

test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.19s

error: test failed, to rerun pass `-p red_knot_test --test inference`
```

---

_Label `red-knot` added by @carljm on 2024-10-05 02:50_

---

_Review requested from @MichaReiser by @carljm on 2024-10-05 02:50_

---

_Review requested from @AlexWaygood by @carljm on 2024-10-05 02:50_

---

_Comment by @github-actions[bot] on 2024-10-05 03:09_

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

_Review comment by @AlexWaygood on `crates/red_knot_test/src/parser.rs`:165 on 2024-10-05 19:06_

nit: this is quite a long method. Could we split it up into `parser_header()`, and `parse_code_block()` methods that are called from the first two branches respectively?

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/parser.rs`:167 on 2024-10-05 19:07_

nit: I'd prefer a more descriptive name like `regex_captures` (or at least `captures` if that's too verbose for you!) rather than `caps`. My first assumption when reading this was that this had something to do with capital letters (which, of course, makes no sense 😆)

---

_Renamed from "[red-knot] test framework" to "[red-knot] type inference/checking test framework" by @carljm on 2024-10-05 19:24_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/parser.rs`:418 on 2024-10-05 19:27_

I'd probably do this like this:

```suggestion
        let mf = super::parse("file.md", &source).expect_err("Should fail to parse");
        assert_eq!(
            mf.to_string(),
            "Header 'Two' not valid inside a test case; parent 'One' has code files.",
        );
```

But I also wonder if using `anyhow` is just causing some unnecessary complications here? Using `Result<(), String>` might be simpler than using `anyhow::Result` for our purposes here. Then we could just do

```suggestion
        assert_eq!(
            mf,
            Err("Header 'Two' not valid inside a test case; parent 'One' has code files.".to_string()),
        );
```

How it's displayed in tracebacks seems fine to me: https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=89dee3e38fce34ead7cb69f282d291b9

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/parser.rs`:113 on 2024-10-05 19:52_

nit: some docs?

```suggestion
/// Matches an arbitrary amount of whitespace (including newlines),
/// followed by a sequence of `#` characters, followed by a title heading,
/// followed by a newlines
static HEADER_RE: Lazy<Regex> =
    Lazy::new(|| Regex::new(r"^(\s*\n)*(?<level>#+)\s+(?<title>.+)\s*\n").unwrap());

/// Matches a code block surrounded by triple backticks,
/// possibly with language and configuration specifications following the
/// opening backticks
static CODE_RE: Lazy<Regex> = Lazy::new(|| {
    Regex::new(r"^```(?<lang>\w+)(?<config>( +\S+)*)\s*\n(?<code>(.|\n)*?)\n```\s*\n").unwrap()
});

/// Matches an arbitrary number of non-newline characters,
/// plus possibly a trailing newline
static IGNORE_RE: Lazy<Regex> = Lazy::new(|| Regex::new(r"^.*\n?").unwrap());
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/parser.rs`:124 on 2024-10-05 19:53_

Could this field maybe just be called `unparsed`?

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/parser.rs`:129 on 2024-10-05 19:54_

Do you mean `Some()` rather than `True`?

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/parser.rs`:340 on 2024-10-05 19:55_

I'd love some docs for this method that make clear that this progresses the "cursor" forward if the passed in pattern matches, but does not if the pattern does not match

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/parser.rs`:195 on 2024-10-05 20:03_

the fact that you have to do this `.last().unwrap()` thing three times in this file, and have to have the same comment next to two of those times explaining why it's safe, makes me wonder if you should create a newtype wrapper for `self.stack`:

```rs
struct SectionStack(Vec<SectionId>);

impl SectionStack {
    fn parent(&self) -> &SectionId {
        self.0.last().expect("Should never pop the implicit root section")
    }
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/parser.rs`:213 on 2024-10-05 20:11_

```suggestion
                let path = config.get("path").copied().unwrap_or("test.py");
```

this means that `path` has type `&str` rather than `&&str`, which then allows you to make these simplifications:

```diff
@@ -220,16 +222,16 @@ impl<'s> Parser<'s> {
                 });
 
                 if let Some(current_files) = &mut self.current_section_files {
-                    if current_files.contains(*path) {
+                    if current_files.contains(path) {
                         let current = &self.sections[*self.stack.last().unwrap()];
                         return Err(anyhow::anyhow!(
                             "Test `{}` has duplicate files named `{path}`.",
                             current.title
                         ));
                     }
-                    current_files.insert(*path);
+                    current_files.insert(path);
                 } else {
-                    self.current_section_files = Some(FxHashSet::from_iter([*path]));
+                    self.current_section_files = Some(FxHashSet::from_iter([path]));
                 }
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/parser.rs`:99 on 2024-10-05 20:16_

If I understand correctly, this struct represents an embedded Python file rather than a MarkDown file? Could you add some docs and/or improve the name to make that a bit clearer?

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/parser.rs`:50 on 2024-10-05 20:19_

```suggestion
        while let Some(parent_id) = parent_id {
            let parent = &self.suite.sections[parent_id];
```

---

_@AlexWaygood reviewed on 2024-10-05 20:34_

Some comments on `parser.rs` (the first commit). Mostly nitpicks. In general, the code looks really well written, but I'd love some more doc-comments for some of these methods :)

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/db.rs`:40 on 2024-10-05 20:39_

nit: could we use a message that will read well using the standard panic hook, as per https://doc.rust-lang.org/std/error/index.html#common-message-styles?

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/db.rs`:9 on 2024-10-05 20:52_

In other crates, we use the name `TestDb` for "fake" `Db` implementations that we use in the test suites for those crates. But here I think this is the "real" `Db` for this crate, right? (I realise the whole crate is just for tests! But this `Db` is for the tests rather than for the tests for the tests. If you get my drift.)

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/assertion.rs`:118 on 2024-10-05 20:56_

nit: could you move this struct and its `impl`s below the `impl`s for `AssertionWithRange`?

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/assertion.rs`:159 on 2024-10-05 20:58_

nit: I'd expect a variable named `line` to hold the contents of that line rather than theh index of that line -- maybe `line_number`?

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/assertion.rs`:216 on 2024-10-05 21:00_

```suggestion
    pub(crate) line_number: OneIndexed,
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/assertion.rs`:226 on 2024-10-05 21:11_

I think some of your `From` implementations would be better as `Deref`s. It would allow you to simplify a bunch of other signatures and remove a bunch of `.into()` calls. All tests pass with this diff:

<details>

```diff
diff --git a/crates/red_knot_test/src/assertion.rs b/crates/red_knot_test/src/assertion.rs
index f55291326..a7b0801ee 100644
--- a/crates/red_knot_test/src/assertion.rs
+++ b/crates/red_knot_test/src/assertion.rs
@@ -219,9 +219,11 @@ pub(crate) struct LineAssertions<'a> {
     pub(crate) assertions: AssertionVec<'a>,
 }
 
-impl<'a, 'b> From<&'a LineAssertions<'b>> for &'a [Assertion<'b>] {
-    fn from(value: &'a LineAssertions<'b>) -> Self {
-        &value.assertions[..]
+impl<'a> Deref for LineAssertions<'a> {
+    type Target = [Assertion<'a>];
+
+    fn deref(&self) -> &Self::Target {
+        &self.assertions
     }
 }
 
diff --git a/crates/red_knot_test/src/diagnostic.rs b/crates/red_knot_test/src/diagnostic.rs
index 7c5f940e6..25a28fea9 100644
--- a/crates/red_knot_test/src/diagnostic.rs
+++ b/crates/red_knot_test/src/diagnostic.rs
@@ -1,6 +1,8 @@
 //! Sort and group diagnostics by line number, so they can be correlated with assertions.
 //!
 //! We don't assume that we will get the diagnostics in source order.
+use std::ops::Deref;
+
 use ruff_source_file::{LineIndex, OneIndexed};
 use ruff_text_size::Ranged;
 use smallvec::SmallVec;
@@ -76,9 +78,11 @@ pub(crate) struct LineDiagnostics<T> {
     pub(crate) diagnostics: DiagnosticVec<T>,
 }
 
-impl<'a, T> From<&'a LineDiagnostics<T>> for &'a [T] {
-    fn from(value: &'a LineDiagnostics<T>) -> Self {
-        &value.diagnostics[..]
+impl<T> Deref for LineDiagnostics<T> {
+    type Target = [T];
+
+    fn deref(&self) -> &Self::Target {
+        &self.diagnostics
     }
 }
 
diff --git a/crates/red_knot_test/src/matcher.rs b/crates/red_knot_test/src/matcher.rs
index 899268bcc..2e3849012 100644
--- a/crates/red_knot_test/src/matcher.rs
+++ b/crates/red_knot_test/src/matcher.rs
@@ -131,8 +131,8 @@ impl Unmatched for Assertion<'_> {
     }
 }
 
-fn unmatched<'a, T: Unmatched + 'a>(unmatched: impl Into<&'a [T]>) -> Vec<String> {
-    unmatched.into().iter().map(Unmatched::unmatched).collect()
+fn unmatched<'a, T: Unmatched + 'a>(unmatched: &'a [T]) -> Vec<String> {
+    unmatched.iter().map(Unmatched::unmatched).collect()
 }
 
 struct Matcher {
@@ -153,15 +153,15 @@ impl Matcher {
     /// Return vector of [`Unmatched`] for any unmatched diagnostics or assertions.
     fn match_line<'a, 'b, T: Diagnostic + 'a>(
         &self,
-        diagnostics: impl Into<&'a [T]>,
-        assertions: impl Into<&'a [Assertion<'b>]>,
+        diagnostics: &'a [T],
+        assertions: &'a [Assertion<'b>],
     ) -> Result<(), Vec<String>>
     where
         'b: 'a,
     {
         let mut failures = vec![];
-        let mut unmatched = diagnostics.into().iter().collect::<Vec<_>>();
-        for assertion in assertions.into() {
+        let mut unmatched: Vec<&T> = diagnostics.iter().collect();
+        for assertion in assertions {
             if !self.matches(assertion, &mut unmatched) {
                 failures.push(assertion.unmatched());
```

</details>

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/assertion.rs`:286 on 2024-10-05 21:17_

huh, is it even worth this being its own type? Maybe the enum could just be

```rs
/// A single diagnostic assertion comment.
#[derive(Debug)]
pub(crate) enum Assertion<'a> {
    /// A `Type: ` assertion.
    Type { expected_type: &'a str },

    /// An `Error: ` assertion.
    Error(ErrorAssertion<'a>),
}
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/assertion.rs`:303 on 2024-10-05 21:19_

```suggestion
        f.write_str("Error:")?;
```

---

_@AlexWaygood reviewed on 2024-10-05 21:21_

Some comments on your second commit. Again, nothing major!

---

_Review comment by @MichaReiser on `Cargo.lock`:2300 on 2024-10-07 09:23_

We should disable the default features of `rstest`. The `futures` dependencies are all pulled in for `async-timeouts` which we don't need (we don't have async tests)

---

_Review comment by @MichaReiser on `Cargo.toml`:294 on 2024-10-07 09:24_

Can we add a comment why `red_knot_test` is ignored or remove it from the ignore list?

---

_Review comment by @MichaReiser on `crates/red_knot_test/Cargo.toml`:27 on 2024-10-07 09:25_

Our convention is to have the ruff/red_knot_crates first because that's how @charliermarsh likes it :P
```suggestion
red_knot_python_semantic = { workspace = true }
red_knot_vendored = { workspace = true }
ruff_db = { workspace = true }
ruff_index = { workspace = true }
ruff_python_trivia = { workspace = true }
ruff_source_file = { workspace = true }
ruff_text_size = { workspace = true }

anyhow = { workspace = true }
once_cell = { workspace = true }
ordermap = { workspace = true }
regex = { workspace = true }
rstest = { workspace = true }
rustc-hash = { workspace = true }
salsa = { workspace = true }
smallvec = { workspace = true }
```

---

_Review comment by @MichaReiser on `crates/red_knot_test/Cargo.toml`:1 on 2024-10-07 09:28_

What's the motivation for making `red_knot_test` its own crate vs making it an integration test by creating a file in `red_knot_python_semantic/tests`?

I could see us having the infrastructure in its own crate but I think the test definition itself should be inside `red_knot_python_semantics/tests` to have clear separation between testing framework and test definition

---

_Review comment by @MichaReiser on `crates/red_knot_test/resources/mdtest/numbers.md`:9 on 2024-10-07 09:28_

We'll end up with a nice mix of `py` and `python` languages :laughing: 

---

_Review comment by @MichaReiser on `crates/red_knot_test/resources/mdtest/numbers.md`:10 on 2024-10-07 09:30_

I'm leaning towards using lower case for pragma comments as this seems more common in the ecosystem. This raises the interesting question if `type:` isn't too close to `type: ignore` and whether we should rather use something else? E.g. `# revealed-type: int`

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/assertion.rs`:74 on 2024-10-07 10:06_

Nit: I know that creating an ad-hoc `Locator` is very common in the formatter but I think it's actually an anti-pattern because it is based on the assumption that `Locator::index` (which is a `LineIndex`) is never used. 

IMO, the proper solution is to separate `LineIndex` and `Locator` because most `Locator` methods don't use the line index. However, that's way beyond the scope of your PR. For now, i suggest using `with_index`.

```suggestion
        CommentRanges::is_own_line(range.start(), &Locator::with_index(&self.source, self.lines.clone()))
```

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/assertion.rs`:73 on 2024-10-07 10:08_

I'm leaning towards changing the signature to `is_own_line_comment(&self, start: TextSize) -> bool` to make it clear that it's a comment range and not any range (e.g. don't pass a node).

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/assertion.rs`:50 on 2024-10-07 10:09_

Nit: It wasn't clear to me what file assertions are. Maybe `InlineAssertions` or `AssertionPragmaComments`?



---

_Review comment by @MichaReiser on `crates/red_knot_test/src/assertion.rs`:107 on 2024-10-07 10:10_

Nit: Use `with_index` or expose a `locator` method on `file_assertions`

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/assertion.rs`:101 on 2024-10-07 10:12_

Can we add some documentation about  what range the  `TextRange` represents? Is it the range of the entire comment or is it the assertion's range?

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/assertion.rs`:125 on 2024-10-07 10:13_

Nit: We could implement this as a `iter` method that returns `impl Iterator<Item=....>` and uses `flat_map` if we don't need to name the type 

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/assertion.rs`:126 on 2024-10-07 10:14_

I don't think it's very common to implement `Deref` for a struct with more than one field. I think I would be surprised by this `Deref` implementation. Also see https://doc.rust-lang.org/std/ops/trait.Deref.html#when-to-implement-deref-or-derefmut

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/assertion.rs`:166 on 2024-10-07 10:17_

We often use backticks for examples so that editors could provide proper highlighting (although most don't)

```suggestion
        // ```python
        // # Error: [unbound-name]
        // # Type: Unbound
        // reveal_type(x)
        // ```
```

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/assertion.rs`:270 on 2024-10-07 10:20_

Nit: The two letter abbreviations are more confusing. What's an `ea` :laughing: 

```suggestion
            Self::Type(assertion) => assertion.fmt(f),
            Self::Error(assertion) => assertion.fmt(f),
```

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/assertion.rs`:229 on 2024-10-07 10:21_

```suggestion
    Lazy::new(|| Regex::new(r"^#\s+Type:\s+(?<ty_display>.+?)\s*$").unwrap());
```

Let's be more opinionated about spacing

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/assertion.rs`:242 on 2024-10-07 10:23_

Nit: I'm normally not very fond of using regex for simple parsing because it has significantly worse performance than manual lexing (eg. by using `Cursor`).  But I think it's fine here. You may want to consider using a `RegexSet` https://docs.rs/regex/latest/regex/struct.RegexSet.html

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/assertion.rs`:151 on 2024-10-07 10:32_

Implementing this all as an iterator is impressive, although I'm not sure if performance matters this much for a testing framework :) 

I don't mind if we keep it the way it is and I'm possibly overlooking something but I would probably have built up the `FileAssertions` struct eagerly by using two `Vector`s and avoided the Iterator complexity. 

```
struct FileAssertions<'a> {
	assertions: Vec<Assertion<'a>>
	lines: Vec<Line>,
	...
}

struct Line {
	number: OneIndexed,
	assertions: Range<usize> // range into `FileAssertions.assertions` 
}
```

This design makes use of the fact that assertions for a single line are in order. This way, we can avoid using `SmallVec` (fewer deps) and never have more than 2 allocations (ignoring resizes)

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/db.rs`:9 on 2024-10-07 10:35_

I think it's a bit of both. For the tests, it's the test db. For the framework, it's the real db...

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/db.rs`:25 on 2024-10-07 10:36_

Nit: `new` can be inlined

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/diagnostic.rs`:4 on 2024-10-07 10:37_

```suggestion
//! We don't assume that we will get the diagnostics in source order.

use ruff_source_file::{LineIndex, OneIndexed};
```

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/diagnostic.rs`:50 on 2024-10-07 10:39_

Could we avoid cloning by returning a reference instead? Or is the lifetime too annoying (I just imagine that we'll have many diagnostics ;))

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/diagnostic.rs`:37 on 2024-10-07 10:40_

Nit: What I like to do for slice iterators instead of using `Peekable` is to clone the slice to peek. Cloning a slice iterator is cheap, it's just two pointers. We also do this in the `Lexer` and I suspect it's cheaper than peeking because `peeking` adds a cost to every `next` call.

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/diagnostic.rs`:68 on 2024-10-07 10:43_

The same as for the assertions. I would probably collect all diagnostics by line and make use of the fact that the diagnostics are sorted. It removes the need for small vec and many of the complex iterator types

```rust
struct LineDiagnostics<T> {
	lines: Vec<{ number; OneIndexed, diagnostics: Range<usize> }>,
	diagnostics: Vec<T>
}
```



---

_Review comment by @MichaReiser on `crates/red_knot_test/src/lib.rs`:12 on 2024-10-07 10:44_

Nit: Could we use a `BTreeMap` or do we need to compare maps by equality/hash them?

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/lib.rs`:38 on 2024-10-07 10:46_

Nit: You could use `camino` to avoid the many `to_str().unwrap` calls.

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/lib.rs`:38 on 2024-10-07 10:49_

This code seems complicated considering that all it does is to strip the `mdtest` prefix. How about using `path.strip_prefix(Path::new(env!("CARGO_MANIFEST_DIR"))`;

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/lib.rs`:40 on 2024-10-07 10:50_

Nit: To make it clear that it isn't the python parser?

```suggestion
    let suite = test_parser::parse(relpath.to_str().unwrap(), &source).unwrap();
```


---

_Review comment by @MichaReiser on `crates/red_knot_test/src/lib.rs`:40 on 2024-10-07 10:51_

It would probably be helpful to have a better error message when parsing the test fails. What's the name of the test file? What's the syntax error?

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/lib.rs`:86 on 2024-10-07 10:53_

We should include the name of the file in the error message or it's unclear where the parser error points to.

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/matcher.rs`:16 on 2024-10-07 11:37_

Nit: Use a `BTreeMap` or a `struct FailuresByLine { lines: Vec<{ number: OneIndexed, failures: Range<usize> }, failures: Vec<String> }` 

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/matcher.rs`:191 on 2024-10-07 11:41_

I agree with @AlexWaygood that the `From` implementations here are surprising. I'm leaning towards `as_slice` methods to make this more explicit.

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/matcher.rs`:217 on 2024-10-07 11:43_

```suggestion
    fn column<T: Ranged>(&self, ranged: &T) -> OneIndexed {
        self.line_index
            .source_location(diagnostic.start(), &self.source)
            .column
    }
```

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/matcher.rs`:197 on 2024-10-07 11:48_

Nit: Collecting to a `FxHashSet` instead of a `Vec` (or `IndexedSet`) simplifies `matches`  and avoids creating `O(assertions)` hash sets.

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/parser.rs`:99 on 2024-10-07 11:49_

I also suggest renaming the struct to avoid naming collisions with `ruff_db::File`. 

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/parser.rs`:16 on 2024-10-07 11:50_

Nit: Consider moving these types next to `Section` and `File`. 

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/parser.rs`:48 on 2024-10-07 11:55_

Nit: I suggest writing the test name last, after inserting the parent names at the beginning. This way you can avoid moving the title by O(parent) times.

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/parser.rs`:94 on 2024-10-07 11:56_

Nit: A u32 or u8 should do. Only use `usize` for types that represent an index or a length. Use fixed integer types otherwise, so that they aren't dependent on CPU architecture.

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/parser.rs`:139 on 2024-10-07 11:58_

I think the `\s\n` can be simplified to using `^` with the multiline modifier (in which case `^` matches the start of a line). The end `\*s\n` can then be simplified to `\s*$`.

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/parser.rs`:209 on 2024-10-07 12:04_

Nit: Should we warn about duplicate configuration keys?

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/parser.rs`:240 on 2024-10-07 12:05_

You can directly use insert since an existing file aborts the parsing anyway
```suggestion
                    if let Some(existing) = current_files.insert(*path) {
                        let current = &self.sections[*self.stack.last().unwrap()];
                        return Err(anyhow::anyhow!(
                            "Test `{}` has duplicate files named `{path}`.",
                            current.title
                        ));
                    }
                } else {
```

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/parser.rs`:244 on 2024-10-07 12:07_

Using a regex to find the end of a line seems overly complicated. It took me a while to figure out what's happening. 

I prefer calling `self.rest.lines()` and updating `self.rest` in place. 

What's even the case where we hit `IGNORE_RE` where it isn't the end? Shouldn't the other patterns match for as long as there are sections / code blocks and the `IGNORE_RE` only gets executed when there are no more sections/code blocks which means we're done and we can set `self.rest = ""`. Should we warn about any content coming after?

---

_Review comment by @MichaReiser on `crates/red_knot_test/tests/mdtest.rs`:1 on 2024-10-07 12:14_

I'm inclined to move the actual tests out of the `red_knot_test` crate and define them in `red_knot_python_semantic` instead. `red_knot_test` then becomes a development dependency of `red_knot_python_semantic` which seems a clearer separation to me (where the `red_knot_test` crate mainly is a testing framework).

---

_@MichaReiser reviewed on 2024-10-07 12:18_

This is great! We should reduce the pulled in dependencies and I suggest moving the tests itself to the `red_knot_python_semantic` crate. See my inline comment. We should also add a `README` explaining the test structure. 

The one design change that I propose is to make the pragma comments lower case because that's the most commonly used style used in the python community and to rename `Type` to `revealed-type` or similar to disambiguate it from `type: ignore`. 

I think it should work as expected but could you add a test for a parenthesized expression:

```python
a = b + (
	# comment 
	5 # revealed-type: Literal[5]
)
```

	

---

_@AlexWaygood reviewed on 2024-10-07 12:24_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/resources/mdtest/numbers.md`:10 on 2024-10-07 12:24_

Hehe, the use of uppercase for these kept on bugging me as well 😆

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/db.rs`:9 on 2024-10-07 12:29_

Still sounds like it could be renamed to make its purpose clearer ;)

---

_@AlexWaygood reviewed on 2024-10-07 12:29_

---

_@AlexWaygood reviewed on 2024-10-07 13:06_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/assertion.rs`:226 on 2024-10-07 13:06_

(If you don't like `Deref`s, then I'd use an `AsRef` implementation or an `as_slice` custom method like Micha suggested, rather than `From`.)

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/diagnostic.rs`:22 on 2024-10-07 13:40_

- Just call `.start()` directly rather than calling `.range().start()`
- use a type annotation rather than the "turbofish" operator

```suggestion
        let mut diagnostics: Vec<_> = diagnostics
            .into_iter()
            .map(|diagnostic| DiagnosticWithLine {
                line: line_index.line_index(diagnostic.start()),
                diagnostic,
            })
            .collect();
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/diagnostic.rs`:53 on 2024-10-07 13:49_

I think you can do this in a slightly more type-safe way that avoids the `.unwrap()` call:

```suggestion
        while let Some(DiagnosticWithLine {
            line: diagnostic_line,
            diagnostic,
        }) = self.inner.peek()
        {
            if diagnostic_line == &line {
                self.inner.next();
                diagnostics.push(diagnostic.clone());
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/diagnostic.rs`:73 on 2024-10-07 13:51_

I would again call this `Line_number` or `line_no` rather than `line`, as I'd expect a `line` field to hold the contents of the line rather than the line's index

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/diagnostic.rs`:87 on 2024-10-07 13:51_

```suggestion
    line_number: OneIndexed,
```

---

_@AlexWaygood reviewed on 2024-10-07 13:51_

Comments on your third commit

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/lib.rs`:12 on 2024-10-07 14:00_

`cargo test -p red_knot_test` passes for me locally with this change to Carl's PR branch:

<details>

```diff
diff --git a/Cargo.lock b/Cargo.lock
index 3f7b29bcc..47ab3a232 100644
--- a/Cargo.lock
+++ b/Cargo.lock
@@ -2244,7 +2244,6 @@ version = "0.0.0"
 dependencies = [
  "anyhow",
  "once_cell",
- "ordermap",
  "red_knot_python_semantic",
  "red_knot_vendored",
  "regex",
diff --git a/crates/red_knot_test/Cargo.toml b/crates/red_knot_test/Cargo.toml
index 8fdaaa4c9..e66000b6a 100644
--- a/crates/red_knot_test/Cargo.toml
+++ b/crates/red_knot_test/Cargo.toml
@@ -13,7 +13,6 @@ license.workspace = true
 [dependencies]
 anyhow = { workspace = true }
 once_cell = { workspace = true }
-ordermap = { workspace = true }
 red_knot_python_semantic = { workspace = true }
 red_knot_vendored = { workspace = true }
 regex = { workspace = true }
diff --git a/crates/red_knot_test/src/lib.rs b/crates/red_knot_test/src/lib.rs
index 6e6e5d4ab..7bd32c522 100644
--- a/crates/red_knot_test/src/lib.rs
+++ b/crates/red_knot_test/src/lib.rs
@@ -1,15 +1,11 @@
-use ordermap::map::OrderMap;
 use red_knot_python_semantic::types::check_types;
 use ruff_db::files::system_path_to_file;
 use ruff_db::parsed::parsed_module;
 use ruff_db::system::{DbWithTestSystem, SystemPathBuf};
-use rustc_hash::FxHasher;
-use std::hash::BuildHasherDefault;
+use std::collections::BTreeMap;
 use std::path::PathBuf;
 
-type FxOrderMap<K, V> = OrderMap<K, V, BuildHasherDefault<FxHasher>>;
-
-type Failures = FxOrderMap<SystemPathBuf, matcher::FailuresByLine>;
+type Failures = BTreeMap<SystemPathBuf, matcher::FailuresByLine>;
 
 mod assertion;
 mod db;
@@ -76,7 +72,7 @@ fn run_test(test: &parser::MarkdownTest) -> Result<(), Failures> {
         system_paths.push(full_path);
     }
 
-    let mut failures = FxOrderMap::default();
+    let mut failures = BTreeMap::default();
 
     for path in system_paths {
         let file = system_path_to_file(&db, path.clone()).unwrap();
diff --git a/crates/red_knot_test/src/matcher.rs b/crates/red_knot_test/src/matcher.rs
index 899268bcc..170010fea 100644
--- a/crates/red_knot_test/src/matcher.rs
+++ b/crates/red_knot_test/src/matcher.rs
@@ -3,7 +3,6 @@
 use crate::assertion::{Assertion, FileAssertions};
 use crate::db::TestDb;
 use crate::diagnostic::SortedDiagnostics;
-use crate::FxOrderMap;
 use red_knot_python_semantic::types::TypeCheckDiagnostic;
 use ruff_db::files::File;
 use ruff_db::source::{line_index, source_text, SourceText};
@@ -11,9 +10,10 @@ use ruff_source_file::{LineIndex, OneIndexed};
 use ruff_text_size::Ranged;
 use rustc_hash::FxHashSet;
 use std::cmp::Ordering;
+use std::collections::BTreeMap;
 use std::sync::Arc;
 
-pub(super) type FailuresByLine = FxOrderMap<OneIndexed, Vec<String>>;
+pub(super) type FailuresByLine = BTreeMap<OneIndexed, Vec<String>>;
```

</details>

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/lib.rs`:71 on 2024-10-07 14:02_

```suggestion
            matches!(file.lang, "py" | "pyi"),
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/matcher.rs`:78 on 2024-10-07 14:09_

this is really nice! love how you did this :D

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/matcher.rs`:247 on 2024-10-07 14:20_

Similar to @MichaReiser's nit elsewhere, I'd prefer `type_assertion`, `t_assertion`, or `ty` here rather than `ta` :-)

---

_@AlexWaygood reviewed on 2024-10-07 14:22_

Comments on your last commit!

---

_@carljm reviewed on 2024-10-07 15:40_

---

_Review comment by @carljm on `Cargo.toml`:294 on 2024-10-07 15:40_

It's ignored because nothing depends on it and it's not a binary, so `cargo shear` thinks that it is unused in the root workspace.

I think the need for this ignore will be eliminated if we move the actual tests to a different red-knot crate, as you suggested elsewhere. Because then that crate will depend on `red_knot_test`.

---

_@carljm reviewed on 2024-10-07 15:44_

---

_Review comment by @carljm on `crates/red_knot_test/Cargo.toml`:1 on 2024-10-07 15:44_

I'm not sure about putting the tests in `red_knot_python_semantic`, because I think eventually (when we add features like incremental tests and configuration and full diagnostic capturing) the tests will depend on (and test!) features of red-knot that live outside `red_knot_python_semantic`, and that could lead to awkward circular dependencies. So I think if we put the tests anywhere other than `red_knot_test`, I would put them in `red_knot` crate probably? I don't have any objection to doing that, it's what I actually did originally, and then it felt simpler to just keep everything together. But I also see the value of separating test framework from tests using it, and if it makes the cargo-shear ignore go away, that's an extra bonus.

---

_@carljm reviewed on 2024-10-07 15:45_

---

_Review comment by @carljm on `crates/red_knot_test/resources/mdtest/numbers.md`:9 on 2024-10-07 15:45_

Not at the moment we won't, because I have an assertion that we only allow `py` and `pyi` 😆 

---

_Review comment by @MichaReiser on `crates/red_knot_test/Cargo.toml`:1 on 2024-10-07 15:51_

I understood that this framework is specific to type inference. For example, we probably want to use something different for lint rules (or at least configure it differently). We intentionally excluded LSP and other cases for now. That's why I would keep the tests local to `red_knot_python_semantic` for now. It gives you a faster iteration cycle because you don't have to recompile all `red_knot` crates. 

I'm not that worried about cyclic dependencies. The new diagnostic system should allow the printing of arbitrary diagnostics without depending on the concrete type (or inspecting them). I can't say whether other extensions to the testing framework will introduce cycles. Still, we can revisit the test placement when we make the extensions (or abstract the logic behind a trait). 



---

_@MichaReiser reviewed on 2024-10-07 15:51_

---

_@carljm reviewed on 2024-10-07 15:51_

---

_Review comment by @carljm on `crates/red_knot_test/resources/mdtest/numbers.md`:10 on 2024-10-07 15:51_

I don't mind going to lowercase. I agree we can't use `type: ` as a prefix; these comments would then look exactly like Python 2 type comments.

I do mind using something really long like `revealed-type` -- we will have a lot of these type assertion comments, and IMO that's too much repetitive typing (and no help from autocomplete, either), and too much visual noise. Originally I wanted this prefix to just be `T: `, more like the mypy framework!

What about `ty: `?

Or I guess we could go with `revealed: ` -- it's longer than I prefer, but I think it's workable.

---

_@MichaReiser reviewed on 2024-10-07 15:51_

---

_Review comment by @MichaReiser on `crates/red_knot_test/resources/mdtest/numbers.md`:9 on 2024-10-07 15:51_

:cry:

---

_@AlexWaygood reviewed on 2024-10-07 15:51_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/Cargo.toml`:1 on 2024-10-07 15:51_

Do all the tests that use this framework _need_ to be in the same crate? Couldn't we have some of the tests using this framework live in the `red_knot_python_semantic` crate and have some of the tests using this framework live in the `red_knot` crate, etc.?

---

_@MichaReiser reviewed on 2024-10-07 15:52_

---

_Review comment by @MichaReiser on `crates/red_knot_test/resources/mdtest/numbers.md`:10 on 2024-10-07 15:52_

I'm fine with `ty` or `revealed`. 


---

_@carljm reviewed on 2024-10-07 15:54_

---

_Review comment by @carljm on `crates/red_knot_test/src/assertion.rs`:50 on 2024-10-07 15:54_

It was meant to be parallel to `LineAssertions`, which are "all assertions on a line" -- this is "all assertions in a (Python) file". Neither of the names you suggested would help make that distinction. I feel like the fact that they are inline pragma-comment assertions is already part of the context of this entire module and clarified in the docstring, so I don't think those facts need repeating in struct names.

I'll at least add more clarity to the doc comment for this struct, and think about if there is any better name.

---

_@AlexWaygood reviewed on 2024-10-07 15:58_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/resources/mdtest/numbers.md`:10 on 2024-10-07 15:58_

I quite like `# revealed:`

---

_@carljm reviewed on 2024-10-07 15:59_

---

_Review comment by @carljm on `crates/red_knot_test/src/assertion.rs`:73 on 2024-10-07 15:59_

I like the clarity but I don't like having to call `.range().start()` at each call site; also, it's not clear to me semantically what it means for a text offset to be "on it's own line". I would maybe rather have the signature just explicitly require an `AssertionWithRange`.

---

_@MichaReiser reviewed on 2024-10-07 16:01_

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/assertion.rs`:73 on 2024-10-07 16:01_

`Ranged` implements `.start()`. No need to write `.range().start()`

>  I would maybe rather have the signature just explicitly require an AssertionWithRange.

I don't mind that

---

_@carljm reviewed on 2024-10-07 16:10_

---

_Review comment by @carljm on `crates/red_knot_test/Cargo.toml`:1 on 2024-10-07 16:10_

Yes, we can use the framework from multiple crates. That wouldn't necessarily help with cyclic deps if the framework itself needs to depend on `red_knot` or `red_knot_workspace` in the future. But I'm ok putting the tests in `red_knot_python_semantic` for now, and seeing how it develops later.

---

_@carljm reviewed on 2024-10-07 16:27_

---

_Review comment by @carljm on `crates/red_knot_test/resources/mdtest/numbers.md`:10 on 2024-10-07 16:27_

I'll go with `# revealed: ` for now and see how we like it in practice. Not hard to change later with a search-and-replace, once we have some experience writing tests.

---

_@carljm reviewed on 2024-10-07 16:28_

---

_Review comment by @carljm on `crates/red_knot_test/src/assertion.rs`:101 on 2024-10-07 16:28_

It's the range of the entire comment, will add doc.

---

_@carljm reviewed on 2024-10-07 16:30_

---

_Review comment by @carljm on `crates/red_knot_test/src/assertion.rs`:126 on 2024-10-07 16:30_

Hmm, after reading over the linked doc, I still feel this is a reasonable `Deref` implementation. An `AssertionWithRange` really _is_ just an `Assertion`, just with a bit of extra metadata that we need for a short time internally. It doesn't seem surprising to me that you can just transparently deref it to an Assertion. 

---

_@carljm reviewed on 2024-10-07 16:34_

---

_Review comment by @carljm on `crates/red_knot_test/src/assertion.rs`:151 on 2024-10-07 16:34_

That's a reasonable design, and maybe it would be simpler. At this point it feels like it would be a major rewrite of this module, and I'm not sure if that's the best use of my time; the current implementation works and I don't think it's so complex that it's a problem.

It's definitely a useful review comment, though -- this pattern (flat vector and ranges into it) is one that still doesn't occur to me quickly as an option.

---

_Review comment by @carljm on `crates/red_knot_test/src/assertion.rs`:226 on 2024-10-07 16:37_

Yes, that's definitely an improvement, thank you!

---

_@carljm reviewed on 2024-10-07 16:37_

---

_@carljm reviewed on 2024-10-07 16:39_

---

_Review comment by @carljm on `crates/red_knot_test/src/assertion.rs`:242 on 2024-10-07 16:39_

Yeah, in this case I chose to prioritize ease of implementation over performance :)

I looked at RegexSet but discarded it as an option because it doesn't support capture groups.

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/assertion.rs`:151 on 2024-10-07 16:40_

With regards to dependencies -- I do actually agree that for a test framework specifically, local build times should possibly actually matter more than the time it actually takes to run the tests. And keeping dependencies down is one way to keep build times low. It's not (yet) nearly as big a problem in red-knot than it is in Ruff, but I find waiting for the tests to finish building far more frustrating when working on Ruff than waiting for the tests to complete once they've started.

If there's an easy way to reduce the number of dependencies that doesn't take a lot of @carljm's time, that might be worth it. (But I agree a major rewrite of the module is probably not worth it.)

---

_@AlexWaygood reviewed on 2024-10-07 16:40_

---

_@carljm reviewed on 2024-10-07 16:43_

---

_Review comment by @carljm on `crates/red_knot_test/src/assertion.rs`:229 on 2024-10-07 16:43_

Hmm. I definitely agree that we should prefer and use the spaced style. It just feels like a confusing/bad experience to silently not treat the comment as an assertion because you left out a space.

If we want the test framework to enforce an opinionated style, I think I'd rather still lex leniently, and then issue an explicit error about the lack of spaces. But then I'm not sure that's a priority right now either :)

---

_@carljm reviewed on 2024-10-07 16:44_

---

_Review comment by @carljm on `crates/red_knot_test/src/assertion.rs`:286 on 2024-10-07 16:44_

Yeah, that's fine, I was just keeping things more parallel, but reducing the overall code is probably better.

---

_@carljm reviewed on 2024-10-07 16:54_

---

_Review comment by @carljm on `crates/red_knot_test/src/assertion.rs`:151 on 2024-10-07 16:54_

Does removing an external dependency that generally won't change between builds reduce build times significantly? I would assume on almost every build we do, smallvec will already be built and won't need rebuilding.

I do agree that build time matters here, though.

It's trivial to remove the smallvec dependency and just use a regular vec instead, but I think that could be a significant cost in test runtime (which I think will also matter, once we accumulate thousands of tests), if I don't do the full refactor to a flat-vector-with-ranges.

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/assertion.rs`:151 on 2024-10-07 16:58_

> Does removing an external dependency that generally won't change between builds reduce build times significantly? I would assume on almost every build we do, smallvec will already be built and won't need rebuilding.

That's true. After the weekly renovate PRs on Mondays a local build generally takes a while longer, and that issue gets worse the more dependencies you have. But that's generally only a weekly issue, so I suppose that's not really too bad either.

---

_@AlexWaygood reviewed on 2024-10-07 16:58_

---

_@carljm reviewed on 2024-10-07 17:00_

---

_Review comment by @carljm on `crates/red_knot_test/src/db.rs`:9 on 2024-10-07 17:00_

I'm not sure whether it makes anything clearer or not, but I have no problem with renaming this to just `Db` -- it is the one and only `Db` for the test framework.

---

_@carljm reviewed on 2024-10-07 17:09_

---

_Review comment by @carljm on `crates/red_knot_test/src/diagnostic.rs`:50 on 2024-10-07 17:09_

I tried this and found the lifetimes difficult to manage. In practice (at least with the current implementation of `TypeCheckDiagnostics`) the concrete type of `T` here is `Arc<TypeCheckDiagnostic>`, so we're just cloning an `Arc`, which isn't too expensive. If that changes in a future re-work of diagnostics and cloning gets more costly, then it might be worth revisiting this to try with references again.

---

_@carljm reviewed on 2024-10-07 17:11_

---

_Review comment by @carljm on `crates/red_knot_test/src/diagnostic.rs`:68 on 2024-10-07 17:11_

Same comment as for assertions: I think the design you propose is a good one, just not sure if I want to spend the time to do that refactor right now.

---

_@carljm reviewed on 2024-10-07 17:15_

---

_Review comment by @carljm on `crates/red_knot_test/src/lib.rs`:38 on 2024-10-07 17:15_

That was my first version. It failed on Windows in CI, with no useful clues in the failure output. I would much rather maintain this code than debug why the `strip_prefix` version failed on Windows 😆 If you want to debug that and then replace this code, I have no problem with that. (Though I would rather strip the prefix all the way up to and including `mdtest`, as this code does, not just strip only the manifest dir.)

---

_@carljm reviewed on 2024-10-07 17:20_

---

_Review comment by @carljm on `crates/red_knot_test/src/matcher.rs`:191 on 2024-10-07 17:20_

I liked Alex's suggestion of using `Deref` here.

---

_@carljm reviewed on 2024-10-07 17:23_

---

_Review comment by @carljm on `crates/red_knot_test/src/matcher.rs`:197 on 2024-10-07 17:23_

It's not clear to me how this helps avoid the need for the `matched` hashset in `matches`. I would assume you still can't pop from an `FxHashSet` while iterating over it?

---

_@carljm reviewed on 2024-10-07 17:28_

---

_Review comment by @carljm on `crates/red_knot_test/src/parser.rs`:94 on 2024-10-07 17:28_

This comes from a `usize` (the number of `#` in the header line); if I use a `u8` or `u32` here it just means I'll have to convert from `usize` (and handle the practically-impossible scenario where it doesn't fit.) Does that still seem worth it to you to avoid `usize` here?

---

_@carljm reviewed on 2024-10-07 17:30_

---

_Review comment by @carljm on `crates/red_knot_test/src/parser.rs`:167 on 2024-10-07 17:30_

I can change it. The regex crate docs use the variable name `caps` for this, so I just kind thought it was a convention; maybe you should take it up with @BurntSushi ;)

---

_Review comment by @BurntSushi on `crates/red_knot_test/src/parser.rs`:167 on 2024-10-07 17:33_

Hah, yeah I like `caps` for this, but it usually appears close to calling a method named `captures` or `captures_iter`, which provides a bit more context.

`captures` might be a good compromise here. I usually (but not always) try to give names a verbosity roughly proportional to the size of their scope.

---

_@BurntSushi reviewed on 2024-10-07 17:33_

---

_@carljm reviewed on 2024-10-07 17:36_

---

_Review comment by @carljm on `crates/red_knot_test/src/parser.rs`:244 on 2024-10-07 17:36_

I want to support arbitrary Markdown prose/markup interspersed in tests, as comments/description (even the sample `numbers.md` included in this PR uses this.) So we need to ignore whatever lines we find in between code blocks and headers.

Splitting by lines makes things a lot more complex because then I can't use a single multi-line regex to match an entire code block, I have to maintain my own state machine for code block parsing. That's possible of course, but I don't think "IGNORE_RE is confusing" is strong enough justification to spend the time on it.

Or maybe you meant using `self.rest.lines()` only in the case the other two regexes didn't match, just to find the next line? I don't think I should split all of `self.rest` in that case, I should just find the next newline. But this seems reasonable; it just means I can't reuse the `scan` method to do the updating.

---

_@carljm reviewed on 2024-10-07 17:40_

---

_Review comment by @carljm on `crates/red_knot_test/src/parser.rs`:418 on 2024-10-07 17:40_

Fine by me; I thought I might end up using anyhow's context feature, but I didn't. Curious if @MichaReiser has any thoughts on `Result<(), String>` vs anyhow.

---

_@MichaReiser reviewed on 2024-10-07 18:56_

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/assertion.rs`:151 on 2024-10-07 18:56_

I'm not too concerned about the smallvec dependency. My main concern is that iterators result in a lot of boilerplate and lifetimes that can be avoided by collecting to a vector. The current design also requires an optimization (small vec) that otherwise wouldn't be necessary. I suspect that it's slightly more code than necessary and refactoring would only require moving some logic around (but not rewriting). 

I do agree with that this might not be the best use of your time to refactor it now. We could consider this a starter task for David, leave it as is and add a TODO, or you try how far you get by spending a limited time on it. 

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/lib.rs`:38 on 2024-10-07 18:59_

Yeah, you would have to join the string with `mdtest` and strip the entire prefix. 

That it fails on windows could be if one of the two paths is absolute and the other is not. 

I suggest adding a TODO or comment explaining why we use this more complex (and not entirely safe) way of stripping a prefix

---

_@MichaReiser reviewed on 2024-10-07 18:59_

---

_@MichaReiser reviewed on 2024-10-07 19:00_

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/matcher.rs`:197 on 2024-10-07 19:00_

I would have do take a closer look again. You can use `retain` which iterators for you

---

_@MichaReiser reviewed on 2024-10-07 19:04_

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/parser.rs`:94 on 2024-10-07 19:04_

Overall yes, because `usize` is intended for sizes that depend on the CPU architecture and handling the error case seems easy enough when using anyhow (`try_from(...).context(...)`) or just use `expect` 

---

_@carljm reviewed on 2024-10-07 19:07_

---

_Review comment by @carljm on `crates/red_knot_test/src/lib.rs`:38 on 2024-10-07 19:07_

I can add a comment. By "not entirely safe", do you mean in case there's an inner directory within `mdtest` also named `mdtest`? I could fix this, but it would make the code even more complex :)

I can also try absolutizing both paths and see if that fixes Windows CI on the `CARGO_MANIFEST_DIR` approach. But I think that might cause problems if we move the tests to a different crate? In general it feels ugly to me to rely on `CARGO_MANIFEST_DIR` in an exported library function.

---

_@MichaReiser reviewed on 2024-10-07 19:07_

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/parser.rs`:244 on 2024-10-07 19:07_

I mean just using `self.rest.lines()` in the case the other two regexes don't match. Although it's unclear to me if that's even necessary or if we can just move to the end because what's an example where the header and code block regex don't match, but they do match after ignoring.

... Or is it because you don't use the multiline flag and this indeed won't be necessary when you use the multiline flag?

---

_@carljm reviewed on 2024-10-07 19:12_

---

_Review comment by @carljm on `crates/red_knot_test/src/matcher.rs`:197 on 2024-10-07 19:12_

I already use `retain` with the vec. The `O(assertions)` hashsets are to track the indices to remove, which I collect while iterating and then later need to use within my call to `retain`.

I would love a better way to do this, but so far I'm not seeing how switching from a vec to a set here will help.

---

_@carljm reviewed on 2024-10-07 19:15_

---

_Review comment by @carljm on `crates/red_knot_test/src/parser.rs`:244 on 2024-10-07 19:15_

I feel like maybe you didn't read the first sentence of my comment :) 

> I want to support arbitrary Markdown prose/markup interspersed in tests, as comments/description (even the sample numbers.md included in this PR uses this.) So we need to ignore whatever lines we find in between code blocks and headers.

---

_@MichaReiser reviewed on 2024-10-07 19:21_

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/lib.rs`:38 on 2024-10-07 19:21_

> I can add a comment. By "not entirely safe", do you mean in case there's an inner directory within mdtest also named mdtest? I could fix this, but it would make the code even more complex :)

Yeah. It's unlikely and not worth fixing. I would rather use strip prefix :P but that's what I had in mind

> In general it feels ugly to me to rely on CARGO_MANIFEST_DIR in an exported library function.

Using `CARGO_MANIFEST_DIR` with a fallback when it isn't set is relatively common. The only challenge is that it needs to run in the crate **calling** the test. 

The counter argument to the current solution is that assuming a test location also feels somewhat ugly for a library function ;)


---

_Review comment by @MichaReiser on `crates/red_knot_test/src/parser.rs`:244 on 2024-10-07 19:22_

I did... it's still unclear in which cases the first and second regex start matching again after skipping over content with the IGNORE regex, at least when using the regex multiline option.

---

_@MichaReiser reviewed on 2024-10-07 19:22_

---

_@carljm reviewed on 2024-10-07 19:25_

---

_Review comment by @carljm on `crates/red_knot_test/src/lib.rs`:38 on 2024-10-07 19:25_

Good point!

I'll look at using strip prefix instead, on the caller side, and see if absolutizing helps Windows. 

---

_@carljm reviewed on 2024-10-07 19:30_

---

_Review comment by @carljm on `crates/red_knot_test/src/parser.rs`:244 on 2024-10-07 19:30_

Oh! Maybe you're right that the combination of multi-line regex option and not anchoring the header/code patterns at start-of-string will be enough to remove the need for explicit ignoring entirely. I will experiment. (And if that doesn't work, I can still avoid using a regex for what is effectively find-next-newline.)

---

_@MichaReiser reviewed on 2024-10-07 19:31_

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/lib.rs`:38 on 2024-10-07 19:31_

Whoops, I forgot to link to the insta code https://github.com/mitsuhiko/insta/blob/ad7baa69bf5fcd54554c81aa97f90c53d1277a2a/insta/src/macros.rs#L27

---

_Review comment by @carljm on `crates/red_knot_test/src/lib.rs`:40 on 2024-10-07 23:26_

The syntax error was already shown by `unwrap`, but the file name was not; good catch!

---

_@carljm reviewed on 2024-10-07 23:26_

---

_@carljm reviewed on 2024-10-07 23:55_

---

_Review comment by @carljm on `crates/red_knot_test/src/lib.rs`:38 on 2024-10-07 23:55_

Looks like the Windows problem is that rstest calls `canonicalize` on each path, so we have to do the same before trying to strip the prefix, so that both paths have the UNC-whatever thingies in them.

Switched back to using `CARGO_MANIFEST_DIR`.

---

_Review comment by @carljm on `crates/red_knot_test/src/matcher.rs`:197 on 2024-10-08 00:19_

I took another look at `Matcher::matches` and was able to rework it slightly to avoid the per-assertion hashset allocation there. I still have `unmatched` as a Vec, though.

---

_@carljm reviewed on 2024-10-08 00:19_

---

_@carljm reviewed on 2024-10-08 01:00_

---

_Review comment by @carljm on `crates/red_knot_test/src/parser.rs`:418 on 2024-10-08 01:00_

There are some places where I'm using `?` that don't work if I switch to just `Result<T, String>` in place of `anyhow`; it doesn't seem worth it to remove the anyhow dep. I changed the error assertions to your recommendation.



---

_@carljm reviewed on 2024-10-08 01:10_

---

_Review comment by @carljm on `crates/red_knot_test/src/parser.rs`:139 on 2024-10-08 01:10_

I don't think "greedy" parsing using the regex multiline option works, at least not cleanly, because it means whichever regex I check first (header or code) will just happily bypass/ignore the other one. We don't ever know whether the next item we expect is a header or a code block.

---

_Review comment by @carljm on `crates/red_knot_test/src/parser.rs`:244 on 2024-10-08 01:18_

As I commented above, multi-line option doesn't work (or at least would require significant changes to the parsing algorithm to make it work) because it's too greedy and will happily bypass things that shouldn't be bypassed. 

I switched to using `.find('\n')` and `.split_at` instead of `IGNORE_RE`.

---

_@carljm reviewed on 2024-10-08 01:18_

---

_Review comment by @carljm on `crates/red_knot_test/src/parser.rs`:240 on 2024-10-08 01:46_

`HashSet::contains` returns a `bool`, not `Option<T>`, but this approach still works.

---

_@carljm reviewed on 2024-10-08 01:46_

---

_Comment by @carljm on 2024-10-08 01:50_

I haven't made it through quite all the comments yet, but you're welcome to review the additional commits I've made so far. I haven't modified the initial commits (other than to rebase on main, which was a clean rebase), just added new ones for various review comments. So you should be able to just review the new commits rather than re-reviewing everything.

---

_@carljm reviewed on 2024-10-08 05:07_

---

_Review comment by @carljm on `crates/red_knot_test/src/assertion.rs`:125 on 2024-10-08 05:07_

In the current iterator-based design, we do need to name the type, because it is a member of `LineAssertionsIterator`.

---

_@carljm reviewed on 2024-10-08 05:22_

---

_Review comment by @carljm on `crates/red_knot_test/src/assertion.rs`:229 on 2024-10-08 05:22_

I don't object to adding a feature to error if there are missing spaces in an assertion comment, but I don't want to do it in this PR. If someone is motivated to do it, or we have trouble enforcing our preferred style without it, we can do it as its own PR.

---

_@carljm reviewed on 2024-10-08 06:28_

---

_Review comment by @carljm on `crates/red_knot_test/src/diagnostic.rs`:68 on 2024-10-08 06:28_

I did this. I don't think it's really simpler; it ends up being significantly more lines of code, not less, and just as many lifetimes. (Maybe you can observe some ways to simplify it further?)

I think it's worth it for diagnostics regardless because we will often have more than one per line (every un-imported `reveal_type`).

---

_@carljm reviewed on 2024-10-08 06:30_

---

_Review comment by @carljm on `crates/red_knot_test/src/assertion.rs`:151 on 2024-10-08 06:30_

I don't think I'm going to do this for assertions in this PR. If someone is motivated to do it in another PR, that's fine, but I'm not really convinced there are strong reasons to do it at all. Based on doing this transformation for diagnostics, I don't see evidence that it will make the code any shorter or simpler. I think it was worth it for diagnostics because we will often have two per line, reducing the effectiveness of the smallvec; I don't think that's true here.

---

_Comment by @carljm on 2024-10-08 06:30_

Ok, now I think all comments are addressed!

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/Cargo.toml`:29 on 2024-10-08 06:35_

This should be a dev dependency

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/parser.rs`:31 on 2024-10-08 06:43_


```suggestion
    pub(crate) fn tests(&self) -> MarkdownTestIterator<'_, 's> {
        MarkdownTestIterator {
            suite: self,
            current_file_index: 0,
        }
    }
}
```

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/parser.rs`:185 on 2024-10-08 06:48_

Nit: The name start together with `finish` implies that there's something in between. I would rename `start` to `parse_impl` or `parse_tests` 

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/parser.rs`:265 on 2024-10-08 06:50_

Nit
```suggestion
                if config.insert(key, val).is_some() {
                    return Err(anyhow::anyhow!("Duplicate config item `{}`.", item));
                }
```

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/matcher.rs`:251 on 2024-10-08 06:58_

Nit: I would use `iter().position()` here, considering that it removes at most one diagnostic:


```suggestion
                let position = unmatched.iter().position(|diagnostic| {
                    !error.rule.is_some_and(|rule| rule != diagnostic.rule())
                        && !error
                            .column
                            .is_some_and(|col| col != self.column(*diagnostic))
                        && !error
                            .message_contains
                            .is_some_and(|needle| !diagnostic.message().contains(needle))
                });
                if let Some(position) = position {
                    // Could we use `swap_remove` here or does the order matter?
                    unmatched.remove(position);
                    true
                } else {
                    false
                }
            }
```

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/matcher.rs`:271 on 2024-10-08 07:03_

Nit, nit:
```suggestion
                let mut matched_revealed_type = None;
                let mut matched_undefined_reveal = None;
                let expected_reveal_type_message = format!("Revealed type is `{expected_type}`");
                for (index, diagnostic) in unmatched.iter().enumerate() {
                    if diagnostic.rule() == "revealed-type"
                        && diagnostic.message() == &expected_reveal_type_message
                    {
                        matched_revealed_type = matched_revealed_type.or(Some(index));
                    } else if diagnostic.rule() == "undefined-reveal" {
                        matched_undefined_reveal = matched_undefined_reveal.or(Some(index));
                    }

                    if matched_revealed_type.is_some() && matched_undefined_reveal.is_some() {
                        break;
                    }
                }
```

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/db.rs`:11 on 2024-10-08 07:05_

Hehe, clever. Can't argue with no prefix being the wrong prefix :D

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/assertion.rs`:36 on 2024-10-08 07:06_


```suggestion
//! ```

use crate::db::Db;
```

---

_@MichaReiser approved on 2024-10-08 07:07_

---

_@AlexWaygood reviewed on 2024-10-08 07:39_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/parser.rs`:418 on 2024-10-08 07:39_

SGTM

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/matcher.rs`:196 on 2024-10-08 08:06_

```suggestion
        let mut unmatched: Vec<_> = diagnostics.iter().collect();
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/matcher.rs`:214 on 2024-10-08 08:07_

```suggestion
            .source_location(ranged.start(), &self.source)
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/diagnostic.rs`:64 on 2024-10-08 08:14_

```suggestion
        if let Some(line_number) = current_line_number {
            diags.lines.push(LineDiagnosticRange {
                line_number,
                diagnostic_range: start..diags.diagnostics.len(),
            });
        }
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/assertion.rs`:256 on 2024-10-08 08:15_

I would probably use a struct variant here:

```suggestion
    Revealed{ expected_ty: &'a str },
```

but no strong opinion

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/parser.rs`:38 on 2024-10-08 08:16_

```suggestion
/// A single MarkDown test inside a MarkDown file
#[derive(Debug)]
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/parser.rs`:70 on 2024-10-08 08:17_

```suggestion
/// Iterator that yields all [`MarkdownTest`]s found inside a [`MarkdownTestSuite`]
#[derive(Debug)]
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/parser.rs`:115 on 2024-10-08 08:20_

I can't remember now I'm skimming over a second time whether this represents a section of a MarkDown file or of a MarkDown test -- some docs would make this clearer!

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/parser.rs`:134 on 2024-10-08 08:20_

```suggestion
/// A "Python file" embedded as a code block in a [`MarkdownTest`]
#[derive(Debug)]
pub(crate) struct EmbeddedFile<'s> {
    section: SectionId,
    pub(crate) path: &'s str,
    pub(crate) lang: &'s str,
    pub(crate) code: &'s str,
}
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/parser.rs`:138 on 2024-10-08 08:22_

Does this have the same invariant as `parent()` below? Is it worth asserting that invariant in debug builds?

```suggestion
    fn pop(&mut self) {
        let popped = self.0.pop();
        debug_assert_ne!(popped, None, "Should never pop the implicit root section");
    }
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/parser.rs`:179 on 2024-10-08 08:24_

Does this parse an entire `MarkdownTestSuite` or just a single `MarkdownTest`? Again, I can't remember now I'm skimming over for a second time; would be great to add a short doc-comment!

---

_@AlexWaygood approved on 2024-10-08 08:24_

A couple more nits, but nothing blocking! This is great!

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/assertion.rs`:1 on 2024-10-08 10:18_

I'm wondering if some or all of this doc-comment should actually be a `README.md` for the crate as a whole. It would be really useful to point contributors to some comprehensive docs on how we write our tests. We could always port an expanded version of this doc-comment to a README as a followup PR, though; this comment of mine isn't blocking.

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/assertion.rs`:124 on 2024-10-08 10:19_

```suggestion
/// Iterator that yields all assertions within a single embedded Python file
#[derive(Debug)]
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/diagnostic.rs`:14 on 2024-10-08 10:33_

```suggestion
/// A container of diagnostics.
///
/// The struct upholds the invariant that all contained diagnostics
/// are sorted by line number.
///
/// The container also keeps track of contiguous ranges of indices
/// within the container. Each index range represents a sub-slice
/// of the contained diagnostics, where the slice of diagnostics is
/// a complete collection of all diagnostics emitted on a single
/// line of source code in an embedded Python file in a MarkDown test.
#[derive(Debug)]
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/diagnostic.rs`:12 on 2024-10-08 10:36_

maybe this would be a slightly clearer field name?

```suggestion
    line_ranges: Vec<LineDiagnosticRange>,
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/diagnostic.rs`:27 on 2024-10-08 10:38_

Is a stable sort important here? No tests fail if I do this:

```suggestion
        diagnostics.sort_unstable_by_key(|diagnostic_with_line| diagnostic_with_line.line_number);
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/diagnostic.rs`:81 on 2024-10-08 10:39_

```suggestion
    diagnostic_index_range: Range<usize>,
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/diagnostic.rs`:116 on 2024-10-08 10:40_

```suggestion
/// All diagnostics that occur on a single line of source
/// code in an embedded Python file in a MarkDown test
#[derive(Debug)]
```

---

_@AlexWaygood reviewed on 2024-10-08 10:44_

Some more nits, mostly on docs and naming. Feel free to respond/tackle some/all of these as followups after landing the PR, nothing blocking

---

_@MichaReiser reviewed on 2024-10-08 10:48_

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/assertion.rs`:1 on 2024-10-08 10:48_

Good point. I mentioned that in my previous review but forgot to check if it's there. I strongly agree that we should document the test format.

---

_@carljm reviewed on 2024-10-08 16:56_

---

_Review comment by @carljm on `crates/red_knot_test/src/parser.rs`:265 on 2024-10-08 16:56_

This seems like it might make a good clippy rule :)

---

_Review comment by @carljm on `crates/red_knot_test/src/matcher.rs`:251 on 2024-10-08 17:00_

Ah, that's much better! TIL about `Iterator::position`. And yeah, we can use `swap_remove` too.

---

_@carljm reviewed on 2024-10-08 17:00_

---

_@carljm reviewed on 2024-10-08 17:12_

---

_Review comment by @carljm on `crates/red_knot_test/src/matcher.rs`:271 on 2024-10-08 17:12_

Ah, thanks for catching the `format!` in loop, that was dumb.

The one bit I didn't take here is removing the `is_none()` checks; I see that `.or(...)` also works fine, and I suspect the perf doesn't matter either way, but I find it a bit clearer to read the logic if we are explicit that we don't need to look for the diagnostics that we already found. The `.or()` is a bit more subtle that we look again, but just don't replace the index if we already have one. 

---

_@carljm reviewed on 2024-10-08 17:16_

---

_Review comment by @carljm on `crates/red_knot_test/src/assertion.rs`:256 on 2024-10-08 17:16_

I dunno, I tried both and I felt like the struct variant was more verbose with no real gain in clarity. There's no ambiguity about what this string could be.

---

_@AlexWaygood reviewed on 2024-10-08 17:18_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/assertion.rs`:256 on 2024-10-08 17:18_

fair enough!

---

_@carljm reviewed on 2024-10-08 17:30_

---

_Review comment by @carljm on `crates/red_knot_test/src/assertion.rs`:1 on 2024-10-08 17:30_

Yeah, I forgot to mention this; I actually already started on a `README.md` for the test framework but it's not done yet. At this point I think I will finish it tomorrow and submit it as a separate PR. Definitely agree this is important.

---

_@carljm reviewed on 2024-10-08 17:36_

---

_Review comment by @carljm on `crates/red_knot_test/src/diagnostic.rs`:27 on 2024-10-08 17:36_

Nope, good call; stable sort is not important. We attach no meaning to the order we receive the diagnostics in.

---

_Comment by @carljm on 2024-10-08 17:41_

Thanks for all the great review comments! Planning to merge once signal is green.

---

_Merged by @carljm on 2024-10-08 19:33_

---

_Closed by @carljm on 2024-10-08 19:33_

---

_Branch deleted on 2024-10-08 19:33_

---
