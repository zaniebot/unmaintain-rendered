```yaml
number: 6616
title: Add support for nested replacements inside format specifications
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: fix/ple1300
created_at: 2023-08-16T14:14:16Z
updated_at: 2023-08-17T14:07:32Z
url: https://github.com/astral-sh/ruff/pull/6616
synced_at: 2026-01-12T02:52:04Z
```

# Add support for nested replacements inside format specifications

---

_Pull request opened by @zanieb on 2023-08-16 14:14_

Closes https://github.com/astral-sh/ruff/issues/6442

Python string formatting like `"hello {place}".format(place="world")` supports format specifications for replaced content such as `"hello {place:>10}".format(place="world")` which will align the text to the right in a container filled up to ten characters. 

Ruff parses formatted strings into `FormatPart`s each of which is either a `Field` (content in `{...}`) or a `Literal` (the normal content). Fields are parsed into name and format specifier sections (we'll ignore conversion specifiers for now).

There are a myriad of specifiers that can be used in a `FormatSpec`. Unfortunately for linters, the specifier values can be dynamically set. For example, `"hello {place:{align}{width}}".format(place="world", align=">", width=10)` and `"hello {place:{fmt}}".format(place="world", fmt=">10")` will yield the same string as before but variables can be used to determine the formatting. In this case, when parsing the format specifier we can't know what _kind_ of specifier is being used as their meaning is determined by both position and value.

Ruff does not support nested replacements and our current data model does not support the concept. Here the data model is updated to support this concept, although linting of specifications with replacements will be inherently limited. We could split format specifications into two types, one without any replacements that we can perform lints with and one with replacements that we cannot inspect. However, it seems excessive to drop all parsing of format specifiers due to the presence of a replacement. Instead, I've opted to parse replacements eagerly and ignore their possible effect on other format specifiers. This will allow us to retain a simple interface for `FormatSpec` and most syntax checks. We may need to add some handling to relax errors if a replacement was seen previously.

It's worth noting that the nested replacement _can_ also include a format specification although it may fail at runtime if you produce an invalid outer format specification. For example, `"hello {place:{fmt:<2}}".format(place="world", fmt=">10")` is valid so we need to represent each nested replacement as a full `FormatPart`.

## Test plan

Adding unit tests for `FormatSpec` parsing and snapshots for PLE1300

---

_Review comment by @MichaReiser on `crates/ruff_python_literal/src/format.rs`:300 on 2023-08-16 14:20_

Nit: I think we can simplify this by moving it outside of the loop and do:

```rust
let Some(text) = text.strip_prefix('{') else {
	return Ok((None, ext));
}
```

---

_Review comment by @MichaReiser on `crates/ruff_python_literal/src/format.rs`:305 on 2023-08-16 14:22_

Would it be possible to inline the `if let Some(pos) = end_bracket_pos)` case by using a return instead of assigning to a variable and using break?

---

_Review comment by @charliermarsh on `crates/ruff_python_literal/src/format.rs`:209 on 2023-08-16 14:23_

My understanding is that different components of the spec can use nested parts. For example, this seems to be valid:

```python
>>> "{0}{1:*^{width}}".format("a", "1", width=15)
'a*******1*******'
```

Is the approach here limited to nested `format_type`?

---

_@charliermarsh reviewed on 2023-08-16 14:23_

---

_@MichaReiser reviewed on 2023-08-16 14:24_

Could you extend the PR summary with what parsing logic you changed? It is difficult to me to review this PR because I'm unfamiliar with format specifications and `format.rs`

---

_@charliermarsh reviewed on 2023-08-16 14:26_

---

_Review comment by @charliermarsh on `crates/ruff_python_literal/src/format.rs`:209 on 2023-08-16 14:26_

I don't know which of the components are allowed to be nested. It might be _any_ of them? E.g., this works too:

```python
>>> "{0}{1:*{width}15}".format("a", "1", width="^")
'a*******1*******'
```

---

_@charliermarsh reviewed on 2023-08-16 14:27_

---

_Review comment by @charliermarsh on `crates/ruff_python_literal/src/format.rs`:209 on 2023-08-16 14:27_

Ah, so does this:

```python
>>> "{0}{1:{width}15}".format("a", "1", width="*^")
'a*******1*******'
```

So it may not even be possible to capture this with this data model, since we can't map from part to nested expression.

---

_Comment by @zanieb on 2023-08-16 14:28_

Ya'll are too fast! I'm still working on this — I should have opened it as a draft but was in the process of writing up the body.

I added support for a replacement at the end of the specification (before the format type) but as Charlie has noted I've realized that we actually need to support _any_ of the items in specification being a replacement. I'll need to consider a bit further the best way to accomplish this.

---

_Converted to draft by @zanieb on 2023-08-16 14:28_

---

_Comment by @charliermarsh on 2023-08-16 14:30_

We might need to just detect that the spec has nested parts, and return some other representation that isn't really analyzable by downstream rules (since we can't know how the spec will be resolved).

---

_@zanieb reviewed on 2023-08-16 16:24_

---

_Review comment by @zanieb on `crates/ruff_python_literal/src/format.rs`:355 on 2023-08-16 16:24_

Would love suggestions here as this feels awkward.

---

_Review comment by @zanieb on `crates/ruff_python_literal/src/format.rs`:305 on 2023-08-16 16:27_

This is copied from the implementation at https://github.com/astral-sh/ruff/blob/8358c80e0e5decf850388b92629e5b04a4ee5f90/crates/ruff_python_literal/src/format.rs#L1068-L1104 — should we change them both?

---

_@zanieb reviewed on 2023-08-16 16:27_

---

_@zanieb reviewed on 2023-08-16 16:30_

---

_Review comment by @zanieb on `crates/ruff_python_literal/src/format.rs`:355 on 2023-08-16 16:30_

An alternative, I guess, is to traverse the entire string _twice_. Once to collect and remove all of the replacements and a second pass on the cleaned text.

---

_@zanieb reviewed on 2023-08-16 16:34_

---

_Review comment by @zanieb on `crates/ruff_python_literal/src/format.rs`:236 on 2023-08-16 16:34_

I think these will always be `FormatPart::Field` should I narrow this type?

---

_Comment by @zanieb on 2023-08-16 16:45_

My decision to just extend `FormatSpec` may not work since all of these `FormatSpec.format_...` methods will drop replacements. I'm unsure of the best approach.

---

_Comment by @charliermarsh on 2023-08-16 16:45_

What do you mean by "drop replacements"? Can you give an example?

---

_Comment by @zanieb on 2023-08-16 16:56_

Sure! The following test will pass

```rust
        assert_eq!(
            FormatSpec::parse("{x}d")
                .unwrap()
                .format_int(&BigInt::from_bytes_be(Sign::Plus, b"\x10")),
            Ok("16".to_owned())
        );
```

but `{x}` could change the formatting and should not necessarily be ignored. Since all the `format_...` methods return results we could just return an error if replacements are present. I'm not sure where these methods are really used though.

---

_Comment by @zanieb on 2023-08-16 16:59_

With https://github.com/astral-sh/ruff/pull/6616/commits/e5d6ec35b568941471e93a0079a15f3511eb6c2c I've updated PLE1300 to lint nested replacements as well.

---

_Comment by @charliermarsh on 2023-08-16 17:01_

Ahhh I see, you're referring to the methods that actually _use_ this to format things. Can we... remove those? I suspect they came from RustPython which needs to support these at runtime. Otherwise, I'd suggest returning an error in those cases.

---

_Comment by @github-actions[bot] on 2023-08-16 17:08_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      3.6±0.04ms    11.3 MB/sec    1.00      3.6±0.03ms    11.4 MB/sec
formatter/numpy/ctypeslib.py               1.00    736.1±7.99µs    22.6 MB/sec    1.00    733.6±3.11µs    22.7 MB/sec
formatter/numpy/globals.py                 1.00     75.9±0.42µs    38.9 MB/sec    1.01     77.0±1.59µs    38.3 MB/sec
formatter/pydantic/types.py                1.00  1454.5±13.98µs    17.5 MB/sec    1.00  1459.2±33.25µs    17.5 MB/sec
linter/all-rules/large/dataset.py          1.00     10.5±0.06ms     3.9 MB/sec    1.00     10.5±0.06ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.02ms     6.0 MB/sec    1.00      2.8±0.01ms     6.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    397.9±0.94µs     7.4 MB/sec    1.00    394.5±2.81µs     7.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.4±0.10ms     4.7 MB/sec    1.00      5.4±0.09ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.00      5.5±0.02ms     7.3 MB/sec    1.01      5.6±0.03ms     7.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1227.4±44.65µs    13.6 MB/sec    1.00  1217.0±13.47µs    13.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    141.0±2.59µs    20.9 MB/sec    1.04    145.9±6.01µs    20.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.01ms    10.3 MB/sec    1.00      2.5±0.02ms    10.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.2±0.10ms     9.7 MB/sec    1.00      4.2±0.08ms     9.7 MB/sec
formatter/numpy/ctypeslib.py               1.00   828.1±20.67µs    20.1 MB/sec    1.00   828.9±17.37µs    20.1 MB/sec
formatter/numpy/globals.py                 1.00     84.7±1.90µs    34.8 MB/sec    1.01     85.6±2.25µs    34.5 MB/sec
formatter/pydantic/types.py                1.00  1669.0±27.39µs    15.3 MB/sec    1.01  1685.2±25.52µs    15.1 MB/sec
linter/all-rules/large/dataset.py          1.00     12.7±0.13ms     3.2 MB/sec    1.05     13.3±0.21ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.04ms     4.8 MB/sec    1.03      3.6±0.06ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    440.2±9.70µs     6.7 MB/sec    1.03   454.8±14.80µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.07ms     3.9 MB/sec    1.05      6.9±0.11ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.09ms     5.8 MB/sec    1.04      7.3±0.12ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1489.7±19.61µs    11.2 MB/sec    1.02  1523.9±27.75µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    176.5±4.46µs    16.7 MB/sec    1.02    180.4±3.75µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.06ms     8.1 MB/sec    1.01      3.2±0.04ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @zanieb on 2023-08-16 17:24_

---

_Comment by @zanieb on 2023-08-16 17:24_

Great that works for me https://github.com/astral-sh/ruff/pull/6624

---

_@zanieb reviewed on 2023-08-16 17:31_

---

_Review comment by @zanieb on `crates/ruff_python_literal/src/format.rs`:240 on 2023-08-16 17:31_

I can switch this out for an access method

---

_Review comment by @zanieb on `crates/ruff/src/rules/pylint/snapshots/ruff__rules__pylint__tests__PLE1300_bad_string_format_character.py.snap`:59 on 2023-08-16 17:31_

I'd really like better range information here. Would that be hard for me to add to `FormatSpec`?

---

_@zanieb reviewed on 2023-08-16 17:31_

---

_@charliermarsh reviewed on 2023-08-16 17:58_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/snapshots/ruff__rules__pylint__tests__PLE1300_bad_string_format_character.py.snap`:59 on 2023-08-16 17:58_

It might be challenging because the thing we get on the left-hand side is the parsed representation which doesn't always match the literal representation. For example, the string on the left could be an implicit concatenation, or it could contain escaped characters, etc...

---

_@charliermarsh reviewed on 2023-08-16 17:59_

---

_Review comment by @charliermarsh on `crates/ruff_python_literal/src/format.rs`:240 on 2023-08-16 17:59_

+1, an access method that returns `&[FormatSpec]` makes sense to me.

---

_Review comment by @charliermarsh on `crates/ruff_python_literal/src/format.rs`:319 on 2023-08-16 18:02_

You can avoid `RefCell` like:

```rust
diff --git a/crates/ruff_python_literal/src/format.rs b/crates/ruff_python_literal/src/format.rs
index ed56457ef..6df394895 100644
--- a/crates/ruff_python_literal/src/format.rs
+++ b/crates/ruff_python_literal/src/format.rs
@@ -316,10 +316,9 @@ fn parse_precision(text: &str) -> Result<(Option<usize>, &str), FormatSpecError>
 
 /// Parses a format part within a format spec
 fn parse_nested_format_part<'a>(
-    parts: &'a RefCell<Vec<FormatPart>>,
+    parts: &mut Vec<FormatPart>,
     text: &'a str,
 ) -> Result<&'a str, FormatSpecError> {
-    let mut parts = parts.borrow_mut();
     let mut end_bracket_pos = None;
     let mut left = String::new();
 
@@ -356,25 +355,25 @@ fn parse_nested_format_part<'a>(
 
 impl FormatSpec {
     pub fn parse(text: &str) -> Result<Self, FormatSpecError> {
-        let replacements = RefCell::new(vec![]);
+        let mut replacements = vec![];
         // get_integer in CPython
-        let text = parse_nested_format_part(&replacements, text)?;
+        let text = parse_nested_format_part(&mut replacements, text)?;
         let (conversion, text) = FormatConversion::parse(text);
-        let text = parse_nested_format_part(&replacements, text)?;
+        let text = parse_nested_format_part(&mut replacements, text)?;
         let (mut fill, mut align, text) = parse_fill_and_align(text);
-        let text = parse_nested_format_part(&replacements, text)?;
+        let text = parse_nested_format_part(&mut replacements, text)?;
         let (sign, text) = FormatSign::parse(text);
-        let text = parse_nested_format_part(&replacements, text)?;
+        let text = parse_nested_format_part(&mut replacements, text)?;
         let (alternate_form, text) = parse_alternate_form(text);
-        let text = parse_nested_format_part(&replacements, text)?;
+        let text = parse_nested_format_part(&mut replacements, text)?;
         let (zero, text) = parse_zero(text);
-        let text = parse_nested_format_part(&replacements, text)?;
+        let text = parse_nested_format_part(&mut replacements, text)?;
         let (width, text) = parse_number(text)?;
-        let text = parse_nested_format_part(&replacements, text)?;
+        let text = parse_nested_format_part(&mut replacements, text)?;
         let (grouping_option, text) = FormatGrouping::parse(text);
-        let text = parse_nested_format_part(&replacements, text)?;
+        let text = parse_nested_format_part(&mut replacements, text)?;
         let (precision, text) = parse_precision(text)?;
-        let text = parse_nested_format_part(&replacements, text)?;
+        let text = parse_nested_format_part(&mut replacements, text)?;
 
         let (format_type, _text) = if text.is_empty() {
             (None, text)
@@ -406,7 +405,7 @@ impl FormatSpec {
             grouping_option,
             precision,
             format_type,
-            replacements: replacements.take(),
+            replacements,
         })
     }
 ```

---

_@charliermarsh reviewed on 2023-08-16 18:02_

---

_@charliermarsh reviewed on 2023-08-16 18:02_

---

_Review comment by @charliermarsh on `crates/ruff_python_literal/src/format.rs`:319 on 2023-08-16 18:02_

I believe the key is to avoid marking the vector as having the same lifetime as the return type. I.e., you want to _avoid_ `parts: &'a mut Vec<FormatPart>`.

---

_@charliermarsh reviewed on 2023-08-16 18:03_

---

_Review comment by @charliermarsh on `crates/ruff_python_literal/src/format.rs`:349 on 2023-08-16 18:03_

`right` is guaranteed to be non-empty because the first character is the closing bracket?

---

_@charliermarsh reviewed on 2023-08-16 18:05_

---

_Review comment by @charliermarsh on `crates/ruff_python_literal/src/format.rs`:892 on 2023-08-16 18:05_

Not for this PR, but I feel like these are good candidates for `insta::assert_debug_snapshot`, so that we don't have to write out and maintain the expectations manually.

---

_Review comment by @charliermarsh on `crates/ruff_python_literal/src/format.rs`:324 on 2023-08-16 18:06_

Nit: move these below the `if text.is_empty()`, to make clear that they're only used in the `else` case?

---

_@charliermarsh reviewed on 2023-08-16 18:06_

---

_@charliermarsh reviewed on 2023-08-16 18:06_

---

_Review comment by @charliermarsh on `crates/ruff_python_literal/src/format.rs`:355 on 2023-08-16 18:06_

A bit tedious but I don't know that I have much better.

---

_@charliermarsh approved on 2023-08-16 18:06_

---

_@charliermarsh reviewed on 2023-08-16 18:07_

---

_Review comment by @charliermarsh on `crates/ruff_python_literal/src/format.rs`:319 on 2023-08-16 18:07_

(We don't actually _need_ multiple and dynamic mutability rules here so it's better to avoid `RefCell`.)

---

_@zanieb reviewed on 2023-08-16 18:11_

---

_Review comment by @zanieb on `crates/ruff_python_literal/src/format.rs`:319 on 2023-08-16 18:11_

Ah interesting the different lifetime was what tripped me up I think, I tried a few variations like this before using the `RefCell`. Thank you!

---

_@zanieb reviewed on 2023-08-16 18:12_

---

_Review comment by @zanieb on `crates/ruff_python_literal/src/format.rs`:892 on 2023-08-16 18:12_

Makes sense, I tried to match the existing pattern but I agree that's more maintainable.

---

_@zanieb reviewed on 2023-08-16 19:27_

---

_Review comment by @zanieb on `crates/ruff_python_literal/src/format.rs`:349 on 2023-08-16 19:27_

I guess? Is `split_at(idx + 1)` clearer? I added a couple test cases and can't make it fail.

---

_@zanieb reviewed on 2023-08-16 19:47_

---

_Review comment by @zanieb on `crates/ruff_python_literal/src/format.rs`:420 on 2023-08-16 19:47_

The former was only used to represent the latter which I found confusing and unnecessarily vague.

---

_@MichaReiser approved on 2023-08-17 06:25_

Related to your question about better ranges. I have the impression that using `Cursor` instead of manually constructing strings and iterating over chars could simplify the implementation significantly (and speed it up!). We could even decide to build a small lexer that tokenized the strings, simplifying the "parsing" part.

---

_Merged by @zanieb on 2023-08-17 14:07_

---

_Closed by @zanieb on 2023-08-17 14:07_

---

_Branch deleted on 2023-08-17 14:07_

---
