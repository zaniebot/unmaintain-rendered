```yaml
number: 6830
title: "Add `LineSuffix` reserved width"
type: pull_request
state: merged
author: cnpryer
labels:
  - formatter
assignees: []
merged: true
base: main
head: line-suffix
created_at: 2023-08-23T23:00:54Z
updated_at: 2023-08-28T11:48:27Z
url: https://github.com/astral-sh/ruff/pull/6830
synced_at: 2026-01-12T02:45:38Z
```

# Add `LineSuffix` reserved width

---

_Pull request opened by @cnpryer on 2023-08-23 23:00_

Closes #5630

### Summary

Currently a trailing, end-of-line comment considered a line suffix element is not measured during formatting. This change introduces a reserved width field to the `ruff_formatter` crate for line suffix elements in order to allow formatter clients to opt-into this behavior.

### Test Plan

Currently just doctest

______
*Outdated*; See [(comment)](https://github.com/astral-sh/ruff/pull/6830#issuecomment-1694420801)
TODO:
- [x] Add `LineSuffix` reserved width [(comment)](https://github.com/astral-sh/ruff/issues/5630#issuecomment-1682406504)
- [x] Add to IR display [(comment)](https://github.com/astral-sh/ruff/pull/6830#discussion_r1303853539)
- [x] Doctest example(s) [(comment)](https://github.com/astral-sh/ruff/pull/6830#discussion_r1306500539)

TODO(optional):
- Use `TextWidth` instead of internal `LineSuffix` [(comment)](https://github.com/astral-sh/ruff/pull/6830#discussion_r1303856995)
- Add benchmark report (this shouldn't impact perf, but see [#6819](https://github.com/astral-sh/ruff/pull/6819) for example)

---

_@cnpryer reviewed on 2023-08-23 23:04_

---

_Review comment by @cnpryer on `crates/ruff_formatter/src/format_element/document.rs`:454 on 2023-08-23 23:04_

Maybe `"line_suffix<width>("`. That might look confusing though.

---

_@cnpryer reviewed on 2023-08-23 23:05_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:175 on 2023-08-23 23:05_

~~collapsed~~

I believe I will need to address this

> crates/ruff_python_formatter/resources/test/fixtures/black/simple_cases/power_op_spacing.py
```diff
--- Black
+++ Ruff
@@ -55,9 +55,11 @@
         view.variance,  # type: ignore[union-attr]
         view.sum_of_weights,  # type: ignore[union-attr]
         out=np.full(view.sum_of_weights.shape, np.nan),  # type: ignore[union-attr]
-        where=view.sum_of_weights**2 > view.sum_of_weights_squared,  # type: ignore[union-attr]
+        where=view.sum_of_weights**2
+        > view.sum_of_weights_squared,  # type: ignore[union-attr]
     )
 
 return np.divide(
-    where=view.sum_of_weights_of_weight_long**2 > view.sum_of_weights_squared,  # type: ignore
+    where=view.sum_of_weights_of_weight_long**2
+    > view.sum_of_weights_squared,  # type: ignore
 )
```

---

_Comment by @github-actions[bot] on 2023-08-23 23:36_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.10      4.5±0.01ms     9.1 MB/sec    1.00      4.1±0.01ms    10.0 MB/sec
formatter/numpy/ctypeslib.py               1.04    886.8±4.31µs    18.8 MB/sec    1.00    850.0±2.79µs    19.6 MB/sec
formatter/numpy/globals.py                 1.01     83.7±1.56µs    35.3 MB/sec    1.00     82.5±0.82µs    35.8 MB/sec
formatter/pydantic/types.py                1.00  1673.8±11.25µs    15.2 MB/sec    1.00   1674.9±4.73µs    15.2 MB/sec
linter/all-rules/large/dataset.py          1.02     10.3±0.09ms     3.9 MB/sec    1.00     10.1±0.03ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      2.7±0.01ms     6.1 MB/sec    1.00      2.7±0.01ms     6.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    381.0±0.63µs     7.7 MB/sec    1.00    379.5±1.22µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.3±0.04ms     4.8 MB/sec    1.00      5.2±0.03ms     4.9 MB/sec
linter/default-rules/large/dataset.py      1.01      5.5±0.03ms     7.4 MB/sec    1.00      5.4±0.02ms     7.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1205.0±8.86µs    13.8 MB/sec    1.00   1187.9±5.21µs    14.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    138.8±0.27µs    21.3 MB/sec    1.01    140.0±2.91µs    21.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.5±0.02ms    10.3 MB/sec    1.00      2.4±0.02ms    10.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.08      5.1±0.04ms     8.0 MB/sec    1.00      4.7±0.05ms     8.6 MB/sec
formatter/numpy/ctypeslib.py               1.03   986.4±11.57µs    16.9 MB/sec    1.00   960.0±11.73µs    17.3 MB/sec
formatter/numpy/globals.py                 1.00     91.1±1.34µs    32.4 MB/sec    1.03     93.5±5.23µs    31.5 MB/sec
formatter/pydantic/types.py                1.00  1892.6±19.35µs    13.5 MB/sec    1.02  1939.0±29.01µs    13.2 MB/sec
linter/all-rules/large/dataset.py          1.00     12.2±0.10ms     3.3 MB/sec    1.01     12.3±0.10ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.03ms     4.9 MB/sec    1.01      3.4±0.03ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    419.0±5.31µs     7.0 MB/sec    1.02    427.1±5.93µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.05ms     4.0 MB/sec    1.01      6.4±0.06ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.04ms     6.0 MB/sec    1.00      6.8±0.04ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1433.8±11.88µs    11.6 MB/sec    1.01  1455.1±20.07µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    166.3±2.32µs    17.7 MB/sec    1.04    172.4±2.43µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.03ms     8.4 MB/sec    1.00      3.0±0.04ms     8.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Add reserved width to `LineSuffix`" to "Add and use reserved width to `LineSuffix`" by @cnpryer on 2023-08-23 23:59_

---

_Renamed from "Add and use reserved width to `LineSuffix`" to "Add and use `LineSuffix` reserved width" by @cnpryer on 2023-08-23 23:59_

---

_Renamed from "Add and use `LineSuffix` reserved width" to "Use `LineSuffix` reserved width" by @cnpryer on 2023-08-24 00:01_

---

_@cnpryer reviewed on 2023-08-24 01:53_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:189 on 2023-08-24 01:53_

Will update this to dup the width calculation to include configured tab width.

---

_@MichaReiser reviewed on 2023-08-24 06:21_

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/format_element/document.rs`:454 on 2023-08-24 06:21_

The most common pattern is to write the property keys first

```
conditional_group(condition: if_group_breaks("#1"), [
  conditional_group(condition: if_group_breaks("#1"), [
    fits_expanded(propagate_expand: true, condition: if_group_fits_on_line("#1"), [
      group(expand: propagated, [
```

https://play.ruff.rs/81156166-0f8b-4f66-b66e-ca5039fca5b1?secondary=FIR

Edit: I would find the information useful because it explains why pragma comments will fit (width 0), but other comments may not (width > 0)

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/format_element/tag.rs`:403 on 2023-08-24 06:25_

Nit: We don't have named structs for most tag variants because this is mostly an internal API, but being able to extract the fields can be useful. But I do see how `StartLineSuffix(u32)` isn't very meaningful.

We could introduce a `TextWidth(u32)` newtype wrapper that also offers `from_text`, `from_char` methods that then allow code-reuse between the Printer and client code.

---

_@MichaReiser reviewed on 2023-08-24 06:25_

---

_@MichaReiser reviewed on 2023-08-24 06:30_

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/printer/mod.rs`:1182 on 2023-08-24 06:30_

This seems correct. `fits` runs when trying to see if groups (and the more advanced best fitting and fill elements) fit before printing. For every group (that has mode =Flat, which is true for most), the printer calls fits to see if it fits on the line. If yes, then the printer prints the group in flat mode, otherwise in expanded mode. The fact that `fits` performs a lookahead means that all state changes are reverted when fits end to ensure that the printer continues printing with the same state as when it called into fits.

The printer has a few optimisations to avoid re-measuring the same groups over and over if it already knows that all of them fit anyway. But for reasoning, it's easiest to assume that the printer does this for every group. 

I hope this helps.

---

_@MichaReiser reviewed on 2023-08-24 06:32_

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/printer/mod.rs`:1699 on 2023-08-24 06:32_

These tests tend to be very painful to write. I like to add examples with doctests to the builders (@charliermarsh's dying because of slow doctests but they are very useful for this) to demonstrate the functioning of IR changes.

---

_@MichaReiser reviewed on 2023-08-24 06:33_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:159 on 2023-08-24 06:33_

It may be worth documenting why using 0 here is fine (because it doesn't matter, so we can avoid doing the computation)

---

_@MichaReiser reviewed on 2023-08-24 06:34_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:175 on 2023-08-24 06:34_

The behavior you have is fine. We want to respect the line width for all comments, but we'll exclude trailing pragma comments in a follow up PR

---

_@MichaReiser reviewed on 2023-08-24 06:35_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:273 on 2023-08-24 06:35_

What's the reason for excluding the line width here or is this just for the moment?

---

_Comment by @MichaReiser on 2023-08-24 06:35_

This is exciting. Thanks for taking the time to work on this!

---

_Label `formatter` added by @MichaReiser on 2023-08-24 06:35_

---

_@cnpryer reviewed on 2023-08-24 11:20_

---

_Review comment by @cnpryer on `crates/ruff_formatter/src/format_element/tag.rs`:403 on 2023-08-24 11:20_

Yea this did feel out of place when I added it. Thanks for the feedback! I really like the `TextWidth` idea.

---

_@cnpryer reviewed on 2023-08-24 11:22_

---

_Review comment by @cnpryer on `crates/ruff_formatter/src/printer/mod.rs`:1699 on 2023-08-24 11:22_

It was a bit tough when I tried adding one. Good to know.

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:273 on 2023-08-24 11:25_

I started by isolating the change to the `ruff_formatter` crate. So, yea, this is likely leftover from that (did this after I struggled with adding a printer test). Thanks for pointing it out.

---

_@cnpryer reviewed on 2023-08-24 11:25_

---

_@cnpryer reviewed on 2023-08-24 13:11_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:159 on 2023-08-24 13:11_

Makes sense.

> because it doesn't matter

To make sure I'm on the same page as you, it doesn't matter because there are no cases where an own-line comment's width needs to be measured, correct?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:159 on 2023-08-24 13:14_

Yes, because we don't split comments, and the empty lines expand any enclosing group. 

---

_@MichaReiser reviewed on 2023-08-24 13:14_

---

_@cnpryer reviewed on 2023-08-24 13:31_

---

_Review comment by @cnpryer on `crates/ruff_formatter/src/printer/mod.rs`:1182 on 2023-08-24 13:31_

Helps a lot. Seems like really interesting logic too, so I'll check it out anyway. Thanks!

---

_@cnpryer reviewed on 2023-08-24 13:39_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:175 on 2023-08-24 13:39_

Removed the comment in 9385e63a278f5e0fbd8f6d78f15605d92e6ba18c

---

_@cnpryer reviewed on 2023-08-24 13:41_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:159 on 2023-08-24 13:41_

Added a comment in 9385e63a278f5e0fbd8f6d78f15605d92e6ba18c but I kinda want to include the detail about splitting as well

EDIT: Updated in 7ae5c3070606eaa992424e9df6a5793f9d7d2d3b

---

_Comment by @cnpryer on 2023-08-24 13:44_

I started updating the PR description to reflect feedback (see todo list). 

As discussed on Discord we can also split this PR up if needed in order to unblock dependent work.

---

_@cnpryer reviewed on 2023-08-24 13:50_

---

_Review comment by @cnpryer on `crates/ruff_formatter/src/printer/mod.rs`:1699 on 2023-08-24 13:50_

I'll mark this as resolved and take note of this. If I land any other printer PRs I'd like to try this again.

---

_Review comment by @cnpryer on `crates/ruff_formatter/src/format_element/tag.rs`:403 on 2023-08-25 14:33_

Would this be something we'd want to add to `ruff_text_size` or similar? Not sure if I'm overthinking it, but I'm trying to understand the `TextSize` struct and how `TextWidth` might relate.

https://github.com/astral-sh/ruff/blob/15b7525464e3f7bd1d28d69a5feb0d50913906e2/crates/ruff_text_size/src/size.rs#L12-L27

This caught my eye
https://github.com/astral-sh/ruff/blob/15b7525464e3f7bd1d28d69a5feb0d50913906e2/crates/ruff_text_size/src/lib.rs#L9-L15

---

_@cnpryer reviewed on 2023-08-25 14:33_

---

_@konstin reviewed on 2023-08-25 15:39_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/format.rs`:165 on 2023-08-25 15:39_

```suggestion
```

---

_@MichaReiser reviewed on 2023-08-25 15:48_

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/format_element/tag.rs`:403 on 2023-08-25 15:48_

I would keep it in `ruff_formatter` for now because it will need access to `TabWidth` which is formatter specific too.

---

_@cnpryer reviewed on 2023-08-25 15:53_

---

_Review comment by @cnpryer on `crates/ruff_formatter/src/format_element/tag.rs`:403 on 2023-08-25 15:53_

Removed the named struct for `StartLineSuffix(u32)` for now. I do want to implement `TextWidth`, but it's possible I'll try to shoot to land a minimum PR instead. If that changes I'll scoop it up here.

In 266c0695d434560e0cb6f55083ebf4ded99ae84a

---

_@cnpryer reviewed on 2023-08-25 15:55_

---

_Review comment by @cnpryer on `crates/ruff_formatter/src/format_element/document.rs`:454 on 2023-08-25 15:55_

Added using `dynamic_text` in 266c0695d434560e0cb6f55083ebf4ded99ae84a. I'll mark this as resolved, but if there's a better way to do it than `std::format!` and the pattern I used I can revise.

Could do `line_suffix(reserved_width: ...)` as well.

---

_@cnpryer reviewed on 2023-08-25 17:35_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:175 on 2023-08-25 17:35_

I see. Didn't realize it flat out ignores all pragmas. Just assumed a format-ignore pragma is what we wanted to handle.  I forgot the pragma placement needs to be retained in order for it to function.

What I imagined was the functionality would be retained with something like this
```py
def fn():
    return np.divide(
        view.variance,
        view.sum_of_weights,
        out=np.full(view.sum_of_weights.shape, np.nan),
        where=(
            view.sum_of_weights**2 > view.sum_of_weights_squared
        ),  # pragma
    )
```

---

_@cnpryer reviewed on 2023-08-26 00:00_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:273 on 2023-08-26 00:00_

Added a `measure_text` helper in 686f3da95e45326e45ac2237b0e6322c3c89f78a.

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:189 on 2023-08-26 00:01_

Duplicated what should be enough for the relevant text in 686f3da95e45326e45ac2237b0e6322c3c89f78a.

---

_@cnpryer reviewed on 2023-08-26 00:01_

---

_Comment by @cnpryer on 2023-08-26 00:32_

Was looking at the ecosystem checks. This is unstable atm.

Pulled from the transformers repo:
```py
class BaseMixedInt8Test(unittest.TestCase):
    # ...
    EXPECTED_RELATIVE_DIFFERENCE = (
        1.540025  # This was obtained on a Quadro RTX 8000 so the number might slightly change
    )
```
```
2023-08-26T00:22:39.664102Z ERROR Unstable formatting /Users/chrispryer/github/ruff/scratch.py
@@ -92,7 +92,9 @@
-    EXPECTED_RELATIVE_DIFFERENCE = 1.540025  # This was obtained on a Quadro RTX 8000 so the number might slightly change
+    EXPECTED_RELATIVE_DIFFERENCE = (
+        1.540025
+    )  # This was obtained on a Quadro RTX 8000 so the number might slightly change
```

From django the same instability occurs with:
```py
class Field:
    hidden_widget = (
        HiddenInput  # Default widget to use when rendering this as "hidden".
    )
```

Would this imply that `black` considers optional parentheses by using the *entire* line-width, including the line-suffix?

FWIW I wanted to just see if this would fix it and it does.
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
<details>

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
</details>

Maybe that's #6771?

---

_Comment by @MichaReiser on 2023-08-26 11:18_

> Would this imply that `black` considers optional parentheses by using the _entire_ line-width, including the line-suffix?

It seems to me that Black removes the parentheses always. This is a file that I used to play around with the formatting.

```python
class BaseMixedInt8Test(unittest.TestCase):
    # ...
    EXPECTED_RELATIVE_DIFFERENCE = (
        1.540025  # This was obtained on a Quadro RTX 8000 so the number might slightly change
    )

    EXPECTED_RELATIVE_DIFFERENCE = 1.540025  # This was obtained on a Quadro RTX 8000 so the number might slightly change

    
    EXPECTED_RELATIVE_DIFFERENCE = (
        1.540025  
    ) # This was obtained on a Quadro RTX 8000 so the number might slightly change

    # ...
    EXPECTED_RELATIVE_DIFFERENCE = (
        1.5  # This was obtained on a Quadro RTX 8000 so the number might slightly change
    )

    EXPECTED_RELATIVE_DIFFERENCE = 1.5  # This was obtained on a Quadro RTX 8000 so the number might slightly change

    
    EXPECTED_RELATIVE_DIFFERENCE = (
        1.5  
    ) # This was obtained on a Quadro RTX 8000 so the number might slightly change
    

    # ...
    EXPECTED_RELATIVE_DIFFERENCE = (
        1.540025  # This was 
    )

    EXPECTED_RELATIVE_DIFFERENCE = 1.540025  # This was

    
    EXPECTED_RELATIVE_DIFFERENCE = (
        1.540025  
    ) # This was
    

    (
        EXPECTED_RELATIVE_DIFFERENCE # This was obtained on a Quadro RTX 8000 so the number might slightly change
    )= (
        1.540025  
    ) # This was

```

I haven't come to a conclusion yet what the best behavior is but we have a few options:

* Keep parentheses if the expression has any trailing comment. This would ensure that comments never move and has low complexity
* Only add parentheses here 
  https://github.com/astral-sh/ruff/blob/813d7da7ec8ab03d397b1496e0c0e99025a72161/crates/ruff_python_formatter/src/expression/mod.rs#L246-L253 
  by changing it to 
  
  ```rust
        group(&format_args![
          text("("),
          soft_block_indent(&format_expression),
          text(")")
      ])
      .with_group_id(Some(group_id))
      .fmt(f)
    ```
    
    This gives us black's formatting in most cases.
* Use your implementation

We'll need to play around with this a little more.

---

_Comment by @codspeed-hq[bot] on 2023-08-26 15:49_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cnpryer:line-suffix)

### Merging #6830 will **not alter performance**

<sub>Comparing <code>cnpryer:line-suffix</code> (f32624c) with <code>main</code> (f33277a)</sub>



### Summary

`✅ 16` untouched benchmarks






---

_Comment by @cnpryer on 2023-08-26 16:09_

Ah I see. I think I was too focused on dealing with the instability. You're right. `black` will remove the parentheses. Now I want to understand the expected behavior a bit better (`black`'s behavior).

So is it specific nodes that are given the "break if line suffix exceeds line-lengths" treatment? On first glance it looks like it occurs where some kind of *parentheses* are present. When I say *parentheses* I mean `{}`, `[]`, or `()`. I'm kind of curious how `black` implements this. I've been kicking the can on reading `black`'s source, so maybe it's time.

Marking as ready to invite some discussion around the solution. I'll continue tinkering.


---

_Marked ready for review by @cnpryer on 2023-08-26 16:10_

---

_Comment by @cnpryer on 2023-08-26 17:44_

I've decided to split this into two PRs. One (this one) will be for adding the new `reserved_width` field to `LineSuffix`. The other (pending) will be for layering in the `ruff_python_formatter` changes.

I wanted to do this because the `LineSuffix` change doesn't need to wait for the remaining decisions here. This would also unblock anyone else from working off the change to close out some of the Alpha work remaining. The `TextWidth` implementation would be a great followup PR that I might throw up in parallel to reading `black` source. And all of this can occur in parallel to the child PR I've split half of this work into.

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/format_element/tag.rs`:67 on 2023-08-26 17:57_

Let's use a named field and add some documentation 
```suggestion
    StartLineSuffix { reserved_width: u32 },
```

---

_@MichaReiser approved on 2023-08-26 17:57_

Looks good to me. My only ask is to use the `struct `syntax for `StartLineSuffix` and document the field. 

---

_@MichaReiser reviewed on 2023-08-26 17:58_

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/builders.rs`:535 on 2023-08-26 17:58_

Oh, something else. Could we add a doctest here that demonstrates the reserved width functionality?

---

_Renamed from "Use `LineSuffix` reserved width" to "Add `LineSuffix` reserved width" by @cnpryer on 2023-08-26 18:01_

---

_@cnpryer reviewed on 2023-08-26 18:01_

---

_Review comment by @cnpryer on `crates/ruff_formatter/src/format_element/tag.rs`:67 on 2023-08-26 18:01_

Oh nice. Definitely better. Added in 999a9a9a35f0d187c21b4adfd519a8b46e7b78b8.

---

_@cnpryer reviewed on 2023-08-26 22:45_

---

_Review comment by @cnpryer on `crates/ruff_formatter/src/builders.rs`:535 on 2023-08-26 22:45_

I'd really like something that's essentially "print the current example with the line suffix on a new line due to its reserved width". Is this possible with something simpler than the following test:
```rust
    #[test]
    fn line_suffix_reserved_width() {
        let options = PrinterOptions {
            print_width: PrintWidth::new(2),
            ..PrinterOptions::default()
        };

        let printed = format_with_options(&format_args![
            group(&format_args![
                &format_with(|f| {
                    f.fill()
                        .entry(
                            &soft_line_break(),
                            &format_args![text("a"), text("b")],
                        )
                        .entry(
                            &soft_line_break(),
                            &line_suffix(&text("c"), 1),
                        )
                        .finish()
                }),
            ]),
        ], options);

        assert_eq!(printed.as_code(), "ab\nc");
    }
```

Added both a doctest and a regular printer test in d67165e92989b10479ecb9509b6cb2aba1be61e9.

---

_@MichaReiser reviewed on 2023-08-27 09:18_

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/builders.rs`:535 on 2023-08-27 09:18_

A very minimalistic line width :D Fill seems a bit overkill. I would create something that's close to what you want when building your own formatter: E.g. adding parentheses when it has a comment:

```rust
let mut context = SimplFormatContext {
	options: SimpleFormatOptions {
		print_width: PrintWidth::try_from(10).unwrap(),
		..SimpleFormatOptions::default()
	},
	..SimpleFormatContext::default()
};

let formatted = format!(context, 	[
	// Breaks
	group(&format_args![
		if_group_breaks(&text("(")),
		soft_block_indent(&format_args![text("a"), line_suffix(&text(" // a comment"), 13]),
		if_group_breaks(&text(")")
	]),

	// Fits
	group(&format_args![
		if_group_breaks(&text("(")),
		soft_block_indent(&format_args![text("a"), line_suffix(&text(" // a comment"), 0]),
		if_group_breaks(&text(")")
	]),
])?;
```

---

_@cnpryer reviewed on 2023-08-27 12:53_

---

_Review comment by @cnpryer on `crates/ruff_formatter/src/builders.rs`:535 on 2023-08-27 12:53_

Fair :) f32624c09e6501b4278b67fc61da5edc737e42ec

FWIW my thought process was if I'm reading the docs I'd be following the example of "push 'c' to end of line using line suffix" to "now push c to end of line if only two characters can fit and see what happens".

---

_@MichaReiser approved on 2023-08-28 05:46_

---

_Merged by @MichaReiser on 2023-08-28 05:46_

---

_Closed by @MichaReiser on 2023-08-28 05:46_

---

_Branch deleted on 2023-08-28 11:48_

---
