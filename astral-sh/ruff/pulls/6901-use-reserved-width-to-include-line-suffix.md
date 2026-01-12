```yaml
number: 6901
title: Use reserved width to include line suffix measurement
type: pull_request
state: merged
author: cnpryer
labels:
  - formatter
assignees: []
merged: true
base: main
head: line-suffix-client
created_at: 2023-08-26T17:54:56Z
updated_at: 2023-08-30T13:17:42Z
url: https://github.com/astral-sh/ruff/pull/6901
synced_at: 2026-01-12T02:45:38Z
```

# Use reserved width to include line suffix measurement

---

_Pull request opened by @cnpryer on 2023-08-26 17:54_

Closes #6771

## Summary

This PR adds logic to measure line suffixes using the new `reserved_width` field added in:
- #6830 

In order to resolve the instability mentioned [here](https://github.com/astral-sh/ruff/pull/6830#issuecomment-1694068456), I've added logic to include parentheses if relevant expressions break (added in e2b6f04716b78bc4fd07edb31b665ca7c2073912). See below for more details on our options.

I made the decision to use option number 2 listed further down only because it had less impact on the current fixtures, and the similarity reports were roughly the same.

## Test Plan

Current fixtures and added the following to the tuple fixtures

```py
i1 = ("aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",)  # This should break
```

And fixtures for https://play.ruff.rs/39e4c196-e81e-4b27-848e-4a535a746def which tests https://github.com/astral-sh/ruff/pull/6901#discussion_r1309088865
____

TODO:
- [x] Calculate width from the client [(comment)](https://github.com/astral-sh/ruff/issues/5630#issuecomment-1682602163)
- [x] Conditional logic [(comment)](https://github.com/astral-sh/ruff/pull/6830#issuecomment-1694268269)

Here are the stability check/similarity results for two potential paths we can take to address the conditional logic needed in order to resolve the ecosystem checks:

1. Retain parentheses if they exist and a trailing comment is found.
```
2023-08-26T01:29:46.973674Z  INFO Finished /Users/chrispryer/github/ruff/target/progress_projects/typeshed: 0 stability errors, 3496 files, similarity index 0.99947), took 1.44s, 0 input files contained syntax errors 
2023-08-26T01:29:48.560901Z  INFO Finished /Users/chrispryer/github/ruff/target/progress_projects/zulip: 0 stability errors, 1437 files, similarity index 0.99938), took 1.59s, 0 input files contained syntax errors 
2023-08-26T01:29:53.443121Z  INFO Finished /Users/chrispryer/github/ruff/target/progress_projects/cpython: 0 stability errors, 1789 files, similarity index 0.76074), took 4.88s, 2 input files contained syntax errors 
2023-08-26T01:29:59.402318Z  INFO Finished /Users/chrispryer/github/ruff/target/progress_projects/transformers: 0 stability errors, 2587 files, similarity index 0.99913), took 5.96s, 13 input files contained syntax errors 
2023-08-26T01:30:00.243994Z  INFO Finished /Users/chrispryer/github/ruff/target/progress_projects/warehouse: 0 stability errors, 648 files, similarity index 0.99657), took 0.84s, 0 input files contained syntax errors 
2023-08-26T01:30:02.818237Z  INFO Finished /Users/chrispryer/github/ruff/target/progress_projects/django: 0 stability errors, 2760 files, similarity index 0.99926), took 2.57s, 1 input files contained syntax errors 
2023-08-26T01:30:02.862987Z  INFO Finished /Users/chrispryer/github/ruff/target/progress_projects/twine: 0 stability errors, 33 files, similarity index 0.99911), took 0.04s, 0 input files contained syntax errors 
2023-08-26T01:30:02.863148Z  INFO Finished: 0 stability errors, 12750 files, tool 17.326796s, 16 input files contained syntax errors 

real	0m34.871s
user	2m9.043s
sys	0m2.703s
+ cat /Users/chrispryer/github/ruff/target/progress_projects_stats.txt
| project      | similarity index |
|--------------|------------------|
| cpython      | 0.76074          |
| django       | 0.99926          |
| transformers | 0.99913          |
| twine        | 0.99911          |
| typeshed     | 0.99947          |
| warehouse    | 0.99657          |
| zulip        | 0.99938          |
```

<details>

<summary>Diff</summary>

```diff
diff --git a/crates/ruff_python_formatter/src/expression/mod.rs b/crates/ruff_python_formatter/src/expression/mod.rs
index 52255ec4f..89121e38d 100644
--- a/crates/ruff_python_formatter/src/expression/mod.rs
+++ b/crates/ruff_python_formatter/src/expression/mod.rs
@@ -183,11 +183,12 @@ impl Format<PyFormatContext<'_>> for MaybeParenthesizeExpression<'_> {
         } = self;
 
         let comments = f.context().comments();
-        let preserve_parentheses = parenthesize.is_optional()
-            && is_expression_parenthesized((*expression).into(), f.context().source());
+        let has_parentheses = is_expression_parenthesized((*expression).into(), f.context().source());
+        let preserve_parentheses = parenthesize.is_optional() && has_parentheses;
 
-        let has_comments =
-            comments.has_leading(*expression) || comments.has_trailing_own_line(*expression);
+        let has_comments = comments.has_leading(*expression)
+            || comments.has_trailing_own_line(*expression)
+            || (has_parentheses && comments.has_trailing(*expression));
```

</details>

2. Only add parentheses if the expression will break 👈 (chosen)
```
2023-08-26T23:30:12.751770Z  INFO Finished /Users/chrispryer/github/ruff/target/progress_projects/typeshed: 0 stability errors, 3496 files, similarity index 0.99947), took 3.30s, 0 input files contained syntax errors 
2023-08-26T23:30:14.374876Z  INFO Finished /Users/chrispryer/github/ruff/target/progress_projects/zulip: 0 stability errors, 1437 files, similarity index 0.99931), took 1.62s, 0 input files contained syntax errors 
2023-08-26T23:30:19.611808Z  INFO Finished /Users/chrispryer/github/ruff/target/progress_projects/cpython: 0 stability errors, 1789 files, similarity index 0.76073), took 5.24s, 2 input files contained syntax errors 
2023-08-26T23:30:26.275173Z  INFO Finished /Users/chrispryer/github/ruff/target/progress_projects/transformers: 0 stability errors, 2587 files, similarity index 0.99913), took 6.66s, 13 input files contained syntax errors 
2023-08-26T23:30:27.026908Z  INFO Finished /Users/chrispryer/github/ruff/target/progress_projects/warehouse: 0 stability errors, 648 files, similarity index 0.99657), took 0.75s, 0 input files contained syntax errors 
2023-08-26T23:30:29.709267Z  INFO Finished /Users/chrispryer/github/ruff/target/progress_projects/django: 0 stability errors, 2760 files, similarity index 0.99925), took 2.68s, 1 input files contained syntax errors 
2023-08-26T23:30:29.755079Z  INFO Finished /Users/chrispryer/github/ruff/target/progress_projects/twine: 0 stability errors, 33 files, similarity index 0.99911), took 0.05s, 0 input files contained syntax errors 
2023-08-26T23:30:29.755205Z  INFO Finished: 0 stability errors, 12750 files, tool 20.303127s, 16 input files contained syntax errors 

real	0m52.809s
user	2m7.643s
sys	0m3.115s
+ cat /Users/chrispryer/github/ruff/target/progress_projects_stats.txt
| project      | similarity index |
|--------------|------------------|
| cpython      | 0.76073          |
| django       | 0.99925          |
| transformers | 0.99913          |
| twine        | 0.99911          |
| typeshed     | 0.99947          |
| warehouse    | 0.99657          |
| zulip        | 0.99931          |
```

<details>

<summary>Diff</summary>

```diff
diff --git a/crates/ruff_python_formatter/src/expression/mod.rs b/crates/ruff_python_formatter/src/expression/mod.rs
index 52255ec4f..363eff950 100644
--- a/crates/ruff_python_formatter/src/expression/mod.rs
+++ b/crates/ruff_python_formatter/src/expression/mod.rs
@@ -247,10 +247,13 @@ impl Format<PyFormatContext<'_>> for MaybeParenthesizeExpression<'_> {
                     if format_expression.inspect(f)?.will_break() {
                         // The group here is necessary because `format_expression` may contain IR elements
                         // that refer to the group id
-                        group(&format_expression)
-                            .with_group_id(Some(group_id))
-                            .should_expand(true)
-                            .fmt(f)
+                        group(&format_args![
+                            text("("),
+                            soft_block_indent(&format_expression),
+                            text(")")
+                        ])
+                        .with_group_id(Some(group_id))
+                        .fmt(f)
                     } else {
                         // Only add parentheses if it makes the expression fit on the line.
                         // Using the flat version as the most expanded version gives a left-to-right splitting behavior
```

</details>

---

_Comment by @codspeed-hq[bot] on 2023-08-26 18:05_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cnpryer:line-suffix-client)

### Merging #6901 will **not alter performance**

<sub>Comparing <code>cnpryer:line-suffix-client</code> (64ee0d6) with <code>main</code> (f33277a)</sub>



### Summary

`✅ 16` untouched benchmarks






---

_Marked ready for review by @cnpryer on 2023-08-26 23:57_

---

_Label `formatter` added by @MichaReiser on 2023-08-27 09:23_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:403 on 2023-08-27 09:24_

Nit: Can we pass `TabWidth` here. Makes the calling code cleaner and enforces that we indeed pass a tab width.

```suggestion
fn measure_text(text: &str, tab_width: TabWidth) -> u32 {
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:291 on 2023-08-27 09:28_

Uh, this is a bit more complicated than I thought it is. Computing the width of the source text is correct in most cases, but not when `format_comment` normalizes the comment. We may need to extract the formatting logic of `format_comment` into a `write_comment` function that returns `FormatResult<u32>` where the `u32` is the length of the formatted comment. 

---

_@MichaReiser requested changes on 2023-08-27 09:29_

Looks good overall but we need to ensure that the comment written by `format_comment` and the width measured by `measure_text` match.

---

_@cnpryer reviewed on 2023-08-27 15:05_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:291 on 2023-08-27 15:05_

Hmm. I see. Makes sense.

Just so I understand better, this means there's no way to track what's written to the buffer on each write operation (as the caller)? If not, would it makes sense to have this kind of abstraction more generally in case you ever want to know how much data was written to the buffer on any write operation?

Originally I was wondering if there was a way to write the data to some intermediate buffer, and then push it to the formatter's buffer. I was hoping I could do that so I could determine exactly how much data is written for each formatted element.

---

_@cnpryer reviewed on 2023-08-27 15:07_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:403 on 2023-08-27 15:07_

Ty for the feedback. I kept swapping between `u32` and `TabWidth` lol. [`diff`](https://github.com/astral-sh/ruff/compare/64ee0d62ea9d02a5b9fd1a8c14ec6b13a16e7821..cfe95874d1e7a33b7e3b5ccfe59efb7b0af39bd9)

---

_@cnpryer reviewed on 2023-08-27 15:37_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:291 on 2023-08-27 15:37_

> We may need to extract the formatting logic of format_comment into a write_comment

Is this possible without a throwaway format context (and a redundant write)? Wouldn't `line_suffix` still need to be provided `FormatComment`? If I'm not missing something, would it be better to have some method on `FormatComment` that gives you the length of the formatted comment? So something like

```rust

let formatted_comment = format_comment(trailing);
let comment_len = formatted_comment.len(); // Determines length with relevant normalizing logic.
write!(
    f,
    [
        line_suffix(
            &format_args![space(), space(), formatted_comment],
            comment_len + 2 // Account for two added spaces
            ),
            expand_parent()
    ]
)?;
``` 
~~Edit: Maybe `formatted_comment.range().len()` to abstract it into range logic (or slice).~~ Nvm this would difficult to align with what would be eventually written.

---

_@cnpryer reviewed on 2023-08-27 18:03_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:291 on 2023-08-27 18:03_

I've added `normalize_comment` from our chat on Discord without any additional refactor (stepping away; I'll come back). a4d21fdcdc80d0409ee5931ea753ae9e2e603886

---

_Converted to draft by @cnpryer on 2023-08-27 18:03_

---

_@cnpryer reviewed on 2023-08-27 18:53_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:435 on 2023-08-27 18:53_

So far `normalize_comment` is basically a duplication of the normalization from `FormatComment`'s `fmt` implementation. I also need to make sure to handle this comment if I'm not already doing so
https://github.com/astral-sh/ruff/blob/fc89976c24d1c8b9d13d88ef70b619e8a2ac393a/crates/ruff_python_formatter/src/comments/format.rs#L317-L318

---

_Marked ready for review by @cnpryer on 2023-08-27 20:00_

---

_@cnpryer reviewed on 2023-08-27 20:02_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:291 on 2023-08-27 20:02_

I decided to go with the `write_comment` idea since I was having trouble refactoring the normalization into a state I was happy with. I didn't want to submit something where there were multiple places to maintain normalization logic.

3186ff15c960668e2f25ae15e8027b1596df99c2

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:435 on 2023-08-27 20:02_

See https://github.com/astral-sh/ruff/pull/6901#discussion_r1306714876

---

_@cnpryer reviewed on 2023-08-27 20:02_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:360 on 2023-08-28 06:39_

The measure comment now formats the comment and then throws it away. This means, every line suffix comment gets formatted twice. We could make some this faster by having a `DropAllBuffer` but formatting the comments twice still seems wasteful. 

That's why I think that `normalize_comments` works better. It has the disadvantage that it needs to allocate a `String` if the comment isn't already formatted (which wasn't the case before), but I think this is neglectable because most comments don't have extra leading whitespace.  

---

_@MichaReiser requested changes on 2023-08-28 06:39_

---

_@cnpryer reviewed on 2023-08-28 12:48_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:360 on 2023-08-28 12:48_

Do you have any feedback on a4d21fdcdc80d0409ee5931ea753ae9e2e603886?

cc https://github.com/astral-sh/ruff/pull/6901#discussion_r1306707331

---

_@MichaReiser reviewed on 2023-08-28 13:09_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:435 on 2023-08-28 13:09_

Yes, but we could change `FormatComment` to use `normalize_comment` internally to avoid the duplication.

---

_Converted to draft by @cnpryer on 2023-08-28 16:46_

---

_@cnpryer reviewed on 2023-08-28 21:02_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:456 on 2023-08-28 21:02_

Working through this then I can do a refactor/clean up.

---

_@cnpryer reviewed on 2023-08-28 21:03_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:435 on 2023-08-28 21:03_

Resolving this to move to https://github.com/astral-sh/ruff/pull/6901#discussion_r1307947658

---

_@cnpryer reviewed on 2023-08-28 21:04_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:324 on 2023-08-28 21:04_

Is this okay or is there a better way to do this?

Edit: See https://github.com/astral-sh/ruff/pull/6901#discussion_r1308276680

---

_@cnpryer reviewed on 2023-08-29 04:23_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:456 on 2023-08-29 04:23_

Note to self I need to add a fixture to catch this https://play.ruff.rs/086fb5b1-f380-4e98-b7aa-1014c90187ae

cc https://github.com/astral-sh/ruff/compare/22a48350befe6ad084434acb54cf5777a0f219e9..cc748357d0071f1d34e609d60ddab8036ea9d9d5

---

_Renamed from "Use reserved width to measure line suffixes" to "Use reserved width to include line suffix measurement" by @cnpryer on 2023-08-29 04:44_

---

_@MichaReiser reviewed on 2023-08-29 06:34_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:324 on 2023-08-29 06:34_

Using `dynamic_text` for both `Cow::Borrowed` and `Cow::Owned` nullifies the benefit of using a `Cow`, because it always allocates a string internally. 

What we want to do here is:

```
match comment {
	Cow::Borrowed(borrowed) => source_text_slice(TextRange::at(start, borrowed.text_len(), ContainsNewlines::No),
	Cow::Owned(owned) => dynamic_text(&owned, Some(start))
}
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:467 on 2023-08-29 06:37_

I believe Black's logic is to not normalize the content in that case, meaning we can return `Cow::Borrowed` 
instead


https://github.com/psf/black/blob/b4dca26c7d93f930bbd5a7b552807370b60d4298/src/black/comments.py#L122-L127

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:181 on 2023-08-29 06:39_

We should write the comment here manually to avoid normalizing it twice. 

We probably want to factor out a helper for writing a line suffix comment:

```rust
trailing_end_of_line_comment(trailing).fmt(f)
```

---

_@MichaReiser reviewed on 2023-08-29 06:39_

---

_@cnpryer reviewed on 2023-08-29 13:11_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:181 on 2023-08-29 13:11_

Thanks! I was thinking about this. I bounced around with a couple ideas, one of them was something like

```rust
pub(crate) const fn normalized_comment(comment_text: &str) -> FormatResult<FormatComment> {
    FormatNormalizedComment { comment_text: normalize_comment(comment)? }
}
```

But `trailing_end_of_line_comment` looks like a better decision.

---

_@cnpryer reviewed on 2023-08-29 16:33_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:467 on 2023-08-29 16:33_

Hmm. I'll check this out. You might be right (thanks for the link), but I'm wondering if since NBSP isn't in the `COMMENT_EXCEPTIONS` you end up with `'# \u{A0}...` anyway [on the next line](https://github.com/psf/black/blob/b4dca26c7d93f930bbd5a7b552807370b60d4298/src/black/comments.py#L128-L129).

I noticed this last night but it was late, so I could have misunderstood lol https://twitter.com/cnpryer/status/1696349192497541504?s=20

cc https://github.com/astral-sh/ruff/pull/6901#discussion_r1308177746

---

_@cnpryer reviewed on 2023-08-29 19:52_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:181 on 2023-08-29 19:52_

First pass here https://github.com/astral-sh/ruff/compare/0aa3a86f1f641c9e67b0efa9f57924a038bb2eef..2f0f844005fa108fe627b21fab62cd1b04fd2c8e  (outdated)

Will do another pass on it to clean things up.

---

_@cnpryer reviewed on 2023-08-29 19:52_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:324 on 2023-08-29 19:52_

Ty!

https://github.com/astral-sh/ruff/compare/0aa3a86f1f641c9e67b0efa9f57924a038bb2eef..2f0f844005fa108fe627b21fab62cd1b04fd2c8e (outdated)

---

_@cnpryer reviewed on 2023-08-29 21:56_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:467 on 2023-08-29 21:56_

I can double check this, but FWIW I returned the comment as-is borrowed:

```diff
diff --git a/crates/ruff_python_formatter/src/comments/format.rs b/crates/ruff_python_formatter/src/comments/format.rs
index 649e80189..756c94ee2 100644
--- a/crates/ruff_python_formatter/src/comments/format.rs
+++ b/crates/ruff_python_formatter/src/comments/format.rs
@@ -452,7 +452,7 @@ fn normalize_comment<'a>(
 
         // Black adds a space before the non-breaking space if part of a type pragma.
         if trimmed.trim_start().starts_with("type:") {
-            return Ok(Cow::Owned(std::format!("# \u{A0}{trimmed}")));
+            return Ok(Cow::Borrowed(comment_text));
         }
```

> crates/ruff_python_formatter/resources/test/fixtures/black/simple_cases/comments_non_breaking_space.py
```
+--- Black␊
++++ Ruff␊
+@@ -8,9 +8,9 @@␊
+ result = 1  # A simple comment␊
+ result = (1,)  # Another one␊
+ ␊
+-result = 1  #  type: ignore␊
++result = 1  # type: ignore␊
+ result = 1  # This comment is talking about type: ignore␊
+-square = Square(4)  #  type: Optional[Square]␊
++square = Square(4)  # type: Optional[Square]␊
+ ␊
+ ␊
+ def function(a: int = 42):␊
```
Job: https://github.com/astral-sh/ruff/actions/runs/6017980492/job/16325332318?pr=6901

---

_Marked ready for review by @cnpryer on 2023-08-29 22:01_

---

_@cnpryer reviewed on 2023-08-29 22:04_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:383 on 2023-08-29 22:04_

Wanted to point this out (coupling with `expand_parent`)

---

_@cnpryer reviewed on 2023-08-29 22:05_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:396 on 2023-08-29 22:05_

Added to eliminate code duplication between `trailing_end_of_line_comment` and `format_comment`

---

_@cnpryer reviewed on 2023-08-29 22:06_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:362 on 2023-08-29 22:06_

I noticed this would eliminate the need of the SAFETY comment/clippy marker

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/tuple.py`:77 on 2023-08-29 23:04_

I was considering adding a dedicated module for comment fixtures. If that's preferred I can move this to one.

---

_@cnpryer reviewed on 2023-08-29 23:04_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/tuple.py`:77 on 2023-08-30 07:39_

I moved it to its own test file. It felt a bit arbitrary to have this in the tuple expression testing. 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:362 on 2023-08-30 07:45_

Using `text_len` gives us the incorrect result because:

* It returns the byte length, not the character width
* It counts tabs with a width of 1 instead of options.tab_width

---

_@MichaReiser approved on 2023-08-30 07:48_

Nice. This greatly improves our black compatibility! 

**This PR**

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76079 |              1789 |              1635 |
| django       |           0.99948 |              2760 |                85 |
| transformers |           0.99925 |              2587 |               495 |
| twine        |           0.99965 |                33 |                 3 |
| typeshed     |           0.99947 |              3496 |              2173 |
| warehouse    |           0.99659 |               648 |                41 |
| zulip        |           0.99938 |              1437 |                47 |


**Base**
| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76082 |              1789 |              1634 |
| django       |           0.99922 |              2760 |               114 |
| transformers |           0.99855 |              2587 |               586 | 
| twine        |           0.99982 |                33 |                 2 |
| typeshed     |           0.99953 |              3496 |              2173 |
| warehouse    |           0.99648 |               648 |                41 |
| zulip        |           0.99928 |              1437 |                54 |


I expect that changing our pragma comment handling will improve the compatibility further. 


---

_Merged by @MichaReiser on 2023-08-30 08:07_

---

_Closed by @MichaReiser on 2023-08-30 08:07_

---

_Branch deleted on 2023-08-30 11:06_

---

_@cnpryer reviewed on 2023-08-30 13:17_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:362 on 2023-08-30 13:17_

Yup. This is my mistake. Thanks for correcting.

---

_@cnpryer reviewed on 2023-08-30 13:17_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/tuple.py`:77 on 2023-08-30 13:17_

Agreed. Thank you.

---
