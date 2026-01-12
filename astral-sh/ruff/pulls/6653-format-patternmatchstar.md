```yaml
number: 6653
title: "Format `PatternMatchStar`"
type: pull_request
state: merged
author: harupy
labels:
  - formatter
assignees: []
merged: true
base: main
head: format-PatternMatchStar
created_at: 2023-08-17T15:30:14Z
updated_at: 2023-08-24T02:26:17Z
url: https://github.com/astral-sh/ruff/pull/6653
synced_at: 2026-01-12T02:45:38Z
```

# Format `PatternMatchStar`

---

_Pull request opened by @harupy on 2023-08-17 15:30_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Close #6558 

## Test Plan

<!-- How was it tested? -->


---

_Comment by @harupy on 2023-08-17 15:34_

As a test, I formatted this code:

```python
x = [1, 2, 3, 4]

match x:
    case [
        1,
        *
        rest,
    ]:
        print(rest)
    case [
        1,
        2,
        *
        # hello
        rest,
    ]:
        print(rest)
    case [*_]:
        print("no match")
```

and got:

```python
x = [1, 2, 3, 4]

match x:
    case [NOT_YET_IMPLEMENTED_PatternMatchSequence, 2]:
        print(rest)
    case [NOT_YET_IMPLEMENTED_PatternMatchSequence, 2]:
        print(rest)
    case [NOT_YET_IMPLEMENTED_PatternMatchSequence, 2]:
        print("no match")
```

How can I test this?

---

_Comment by @github-actions[bot] on 2023-08-17 16:03_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.6±0.04ms    11.3 MB/sec    1.01      3.7±0.03ms    11.1 MB/sec
formatter/numpy/ctypeslib.py               1.00    732.8±2.47µs    22.7 MB/sec    1.03    755.2±2.78µs    22.0 MB/sec
formatter/numpy/globals.py                 1.00     74.6±0.49µs    39.6 MB/sec    1.08     80.2±0.34µs    36.8 MB/sec
formatter/pydantic/types.py                1.00   1452.6±6.26µs    17.6 MB/sec    1.03  1501.2±11.63µs    17.0 MB/sec
linter/all-rules/large/dataset.py          1.00     10.2±0.05ms     4.0 MB/sec    1.00     10.2±0.04ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.00ms     6.1 MB/sec    1.00      2.7±0.01ms     6.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    386.5±0.71µs     7.6 MB/sec    1.00    386.0±0.54µs     7.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.3±0.03ms     4.8 MB/sec    1.00      5.3±0.02ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.00      5.4±0.02ms     7.5 MB/sec    1.00      5.4±0.02ms     7.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1202.9±18.95µs    13.8 MB/sec    1.00   1207.8±1.23µs    13.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    140.8±1.14µs    21.0 MB/sec    1.00    140.2±1.03µs    21.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.01ms    10.4 MB/sec    1.01      2.5±0.01ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.0±0.13ms     8.1 MB/sec    1.03      5.1±0.26ms     7.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1011.0±60.02µs    16.5 MB/sec    1.00  1011.4±46.37µs    16.5 MB/sec
formatter/numpy/globals.py                 1.00    102.3±7.89µs    28.9 MB/sec    1.03    105.3±6.89µs    28.0 MB/sec
formatter/pydantic/types.py                1.00      2.0±0.14ms    12.5 MB/sec    1.01      2.1±0.08ms    12.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.4±0.31ms     2.6 MB/sec    1.01     15.6±0.34ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.33ms     3.9 MB/sec    1.00      4.2±0.12ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   522.6±20.53µs     5.6 MB/sec    1.02   531.2±22.75µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.0±0.20ms     3.2 MB/sec    1.03      8.3±0.27ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.21ms     4.7 MB/sec    1.02      8.8±0.22ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1828.4±50.56µs     9.1 MB/sec    1.02  1866.5±52.73µs     8.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   213.1±12.28µs    13.8 MB/sec    1.04   221.7±18.09µs    13.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.13ms     6.6 MB/sec    1.03      4.0±0.31ms     6.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @harupy on 2023-08-17 16:23_

Tweaked the `PatternMatchSequence` (should #6645 be resolved first?) implementation and got this:

```python
x = [1, 2, 3, 4]

match x:
    case [
        "NOT_YET_IMPLEMENTED_PatternMatchValue",
        *rest,
    ]:
        print("a")
        print(rest)
    case [
        "NOT_YET_IMPLEMENTED_PatternMatchValue",
        "NOT_YET_IMPLEMENTED_PatternMatchValue",
        *rest# hello
        ,
    ]:
        print(rest)
    case [*_]:
        print("no match")

```

The comment location seems incorrect.

---

_@harupy reviewed on 2023-08-17 17:04_

---

_Review comment by @harupy on `crates/ruff_python_formatter/src/pattern/pattern_match_star.rs`:14 on 2023-08-17 17:04_

Should comments be handled in `PatternMatchSequence`?

---

_Comment by @MichaReiser on 2023-08-18 07:45_

Hmm yeah, it may be challenging to test this just now without match as, singleton formatting, or sequence formatting. There are PRs for `as` and `singleton` but it may be good to do sequence first.

---

_@MichaReiser reviewed on 2023-08-18 07:47_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/pattern/pattern_match_star.rs`:14 on 2023-08-18 07:47_

It depends on the comments. What's the code snipped that requires the dangling comment handling? 

---

_@harupy reviewed on 2023-08-18 11:41_

---

_Review comment by @harupy on `crates/ruff_python_formatter/src/pattern/pattern_match_star.rs`:14 on 2023-08-18 11:41_

@MichaReiser 

For example:

```python
x = [1, 2, 3, 4]

match x:
    case [
        1,
        *
        # comment here
        rest,
    ]:
        print(rest)
```

---

_Comment by @harupy on 2023-08-18 11:41_

> Hmm yeah, it may be challenging to test this just now without match as, singleton formatting, or sequence formatting. There are PRs for `as` and `singleton` but it may be good to do sequence first.

Can I tackle #6645 first?

---

_Comment by @MichaReiser on 2023-08-18 11:59_

> > Hmm yeah, it may be challenging to test this just now without match as, singleton formatting, or sequence formatting. There are PRs for `as` and `singleton` but it may be good to do sequence first.
> 
> Can I tackle #6645 first?

Sure. You'll have to comment on the issue so that I can assign it to you.

---

_Label `formatter` added by @konstin on 2023-08-21 08:32_

---

_Marked ready for review by @harupy on 2023-08-23 09:56_

---

_Review comment by @konstin on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/match.py`:280 on 2023-08-23 10:46_

Could you add test case for the following?
```python
    case [1, 2, * # comment
        rest]:
        pass
    case [1, 2, * # comment
        _]:
        pass
```

---

_Review comment by @konstin on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/match.py`:46 on 2023-08-23 10:47_

This should be uncommented before merging

---

_@konstin approved on 2023-08-23 10:48_

---

_@harupy reviewed on 2023-08-23 10:51_

---

_Review comment by @harupy on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/match.py`:46 on 2023-08-23 10:51_

Forgot to uncomment this, thanks for the catch!

---

_@harupy reviewed on 2023-08-23 10:56_

---

_Review comment by @harupy on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/match.py`:280 on 2023-08-23 10:56_

Added!

---

_@harupy reviewed on 2023-08-23 10:56_

---

_Review comment by @harupy on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/match.py`:286 on 2023-08-23 10:56_

This is formatted first to:

```python
    case [
        "NOT_YET_IMPLEMENTED_PatternMatchValue",
        "NOT_YET_IMPLEMENTED_PatternMatchValue",
        *rest  # comment
        ,
    ]:
        pass
    case [
        "NOT_YET_IMPLEMENTED_PatternMatchValue",
        "NOT_YET_IMPLEMENTED_PatternMatchValue",
        *_  # comment
        ,
    ]:
        pass
```

then formatted to:

```python
    case [
        "NOT_YET_IMPLEMENTED_PatternMatchValue",
        "NOT_YET_IMPLEMENTED_PatternMatchValue",
        *rest,  # comment
    ]:
        pass
    case [
        "NOT_YET_IMPLEMENTED_PatternMatchValue",
        "NOT_YET_IMPLEMENTED_PatternMatchValue",
        *_,  # comment
    ]:
        pass
```

---

_@konstin reviewed on 2023-08-23 12:11_

---

_Review comment by @konstin on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/match.py`:286 on 2023-08-23 12:11_

If you look at
```python
match foo:
    case [* # comment
        rest, 1]:
        pass
    case [*rest # comment
          , 1]:
        pass
```
you get
```
{
    Node {
        kind: PatternMatchStar,
        range: 21..45,
        source: `* # comment⏎`,
    }: {
        "leading": [],
        "dangling": [
            SourceComment {
                text: "# comment",
                position: EndOfLine,
                formatted: true,
            },
        ],
        "trailing": [],
    },
    Node {
        kind: PatternMatchStar,
        range: 74..79,
        source: `*rest`,
    }: {
        "leading": [],
        "dangling": [],
        "trailing": [
            SourceComment {
                text: "# comment",
                position: EndOfLine,
                formatted: true,
            },
        ],
    },
}
```
so i think the best solution is to manually reassign the comment from dangling to trailing.

---

_@harupy reviewed on 2023-08-23 12:42_

---

_Review comment by @harupy on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/match.py`:286 on 2023-08-23 12:42_

@konstin Thanks for the comment. How can we access this leading-dangiling-trailing mapping?

---

_@charliermarsh reviewed on 2023-08-23 13:54_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/match.py`:286 on 2023-08-23 13:54_

@harupy - I typically run something like `cargo run foo.py --emit=stdout --print-comments` from `ruff/crates/ruff_python_formatter`.

---

_@charliermarsh reviewed on 2023-08-23 13:55_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/match.py`:286 on 2023-08-23 13:55_

Oh sorry. You're asking how to modify these assignments. Any such modifications need to be done in `placement.rs` -- those are essentially custom rules that let us change the association of comments. See, e.g., `handle_trailing_expression_starred_star_end_of_line_comment` which is somewhat similar.

---

_@konstin reviewed on 2023-08-23 13:59_

---

_Review comment by @konstin on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/match.py`:286 on 2023-08-23 13:59_

You have to add a case in `handle_enclosed_comment` for when the enclosing node is `PatternMatchStar`. The general logic to fix comment placement is in `placement.rs`/`place_comment`.

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/placement.rs`:1203 on 2023-08-23 15:44_

You can get the `ast::PatternMatchStar` from the `AnyNodeRef::PatternMatchStar(_)` in the match and then pass it to the function (like `handle_leading_class_with_decorators_comment`)

---

_@konstin reviewed on 2023-08-23 15:44_

---

_Review comment by @harupy on `crates/ruff_python_formatter/src/comments/placement.rs`:1203 on 2023-08-23 15:55_

```diff
diff --git a/crates/ruff_python_formatter/src/comments/placement.rs b/crates/ruff_python_formatter/src/comments/placement.rs
index 01dc6b905..a367e6043 100644
--- a/crates/ruff_python_formatter/src/comments/placement.rs
+++ b/crates/ruff_python_formatter/src/comments/placement.rs
@@ -206,7 +206,7 @@ fn handle_enclosed_comment<'a>(
         }
         AnyNodeRef::WithItem(_) => handle_with_item_comment(comment, locator),
         AnyNodeRef::PatternMatchAs(_) => handle_pattern_match_as_comment(comment, locator),
-        AnyNodeRef::PatternMatchStar(_) => handle_pattern_match_star_comment(comment),
+        AnyNodeRef::PatternMatchStar(range, ..) => CommentPlacement::trailing(range, comment),
         AnyNodeRef::StmtFunctionDef(_) => handle_leading_function_with_decorators_comment(comment),
         AnyNodeRef::StmtClassDef(class_def) => {
             handle_leading_class_with_decorators_comment(comment, class_def)
```

Can we just do this?

---

_@harupy reviewed on 2023-08-23 15:55_

---

_@harupy reviewed on 2023-08-23 15:58_

---

_Review comment by @harupy on `crates/ruff_python_formatter/src/comments/placement.rs`:1203 on 2023-08-23 15:58_

Ah I see, this doesn't work for the following case:

```python
    case [
        1,
        *
        # comment
        rest,
    ]:
        pass
```

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1203 on 2023-08-23 16:50_

I think I'd suggest treating these as dangling, and then rendering them as leading comments within the node.

---

_@charliermarsh reviewed on 2023-08-23 16:50_

---

_@charliermarsh reviewed on 2023-08-23 16:58_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1203 on 2023-08-23 16:58_

(Leading is more consistent with how we treat comments in starred _expressions_.)

---

_@charliermarsh reviewed on 2023-08-24 01:49_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1203 on 2023-08-24 01:49_

(Tweaked this to merge.)

---

_Merged by @charliermarsh on 2023-08-24 01:58_

---

_Closed by @charliermarsh on 2023-08-24 01:58_

---

_Branch deleted on 2023-08-24 01:58_

---

_Comment by @harupy on 2023-08-24 01:59_

@charliermarsh Thanks for the update and merging!

---

_Comment by @charliermarsh on 2023-08-24 02:26_

Thanks @harupy for another great contribution!

---
