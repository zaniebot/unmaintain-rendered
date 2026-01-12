```yaml
number: 6274
title: "Add formatter support for call and class definition `Arguments`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/class-base-format
created_at: 2023-08-02T13:58:40Z
updated_at: 2023-08-03T20:09:45Z
url: https://github.com/astral-sh/ruff/pull/6274
synced_at: 2026-01-12T02:52:03Z
```

# Add formatter support for call and class definition `Arguments`

---

_Pull request opened by @charliermarsh on 2023-08-02 13:58_

## Summary

This PR leverages the `Arguments` AST node introduced in #6259 in the formatter, which ensures that we correctly handle trailing comments in calls, like:

```python
f(
  1,
  # comment
)

pass
```

(Previously, this was treated as a leading comment on `pass`.)

This also allows us to unify the argument handling across calls and class definitions.

## Test Plan

A bunch of new fixture tests, plus improved Black compatibility.


---

_Label `formatter` added by @charliermarsh on 2023-08-02 13:58_

---

_Review requested from @konstin by @charliermarsh on 2023-08-02 13:58_

---

_@charliermarsh reviewed on 2023-08-02 13:59_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/builders.rs`:255 on 2023-08-02 13:59_

We don't want these to break, IIUC.

---

_@charliermarsh reviewed on 2023-08-02 13:59_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/stmt_class_def.rs`:79 on 2023-08-02 13:59_

We get all of this for free now, I think.

---

_@charliermarsh reviewed on 2023-08-02 13:59_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/other/arguments.rs`:19 on 2023-08-02 13:59_

This is largely moved from `expr_call`.

---

_@zanieb approved on 2023-08-02 14:03_

---

_Comment by @charliermarsh on 2023-08-02 14:21_

Looking into ecosystem failure.

---

_Converted to draft by @charliermarsh on 2023-08-02 14:28_

---

_Comment by @charliermarsh on 2023-08-02 14:30_

(Need to fix a few things.)

---

_Comment by @github-actions[bot] on 2023-08-02 14:41_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.5±0.05ms     4.8 MB/sec    1.01      8.6±0.07ms     4.7 MB/sec
formatter/numpy/ctypeslib.py               1.01  1661.0±36.23µs    10.0 MB/sec    1.00  1646.3±14.92µs    10.1 MB/sec
formatter/numpy/globals.py                 1.00    172.1±4.74µs    17.1 MB/sec    1.02    174.7±5.03µs    16.9 MB/sec
formatter/pydantic/types.py                1.00      3.6±0.09ms     7.0 MB/sec    1.00      3.6±0.06ms     7.0 MB/sec
linter/all-rules/large/dataset.py          1.00     11.6±0.09ms     3.5 MB/sec    1.00     11.6±0.10ms     3.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.0±0.02ms     5.6 MB/sec    1.00      3.0±0.02ms     5.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    319.1±2.21µs     9.2 MB/sec    1.00    318.5±1.63µs     9.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.2±0.06ms     4.9 MB/sec    1.00      5.2±0.04ms     4.9 MB/sec
linter/default-rules/large/dataset.py      1.01      6.2±0.05ms     6.6 MB/sec    1.00      6.1±0.03ms     6.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1215.2±6.93µs    13.7 MB/sec    1.00   1215.8±4.55µs    13.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    120.7±0.24µs    24.5 MB/sec    1.00    121.0±0.31µs    24.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.7±0.01ms     9.6 MB/sec    1.00      2.7±0.02ms     9.6 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      9.9±0.07ms     4.1 MB/sec     1.00      9.9±0.08ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1929.6±100.51µs     8.6 MB/sec    1.00  1925.1±26.59µs     8.6 MB/sec
formatter/numpy/globals.py                 1.02    215.8±5.35µs    13.7 MB/sec     1.00    212.2±4.75µs    13.9 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.04ms     6.1 MB/sec     1.00      4.2±0.07ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.3±0.07ms     3.1 MB/sec     1.00     13.3±0.14ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.03ms     4.7 MB/sec     1.00      3.5±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    435.1±5.88µs     6.8 MB/sec     1.00    435.7±7.83µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.07ms     4.2 MB/sec     1.00      6.0±0.07ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.04ms     5.6 MB/sec     1.01      7.3±0.05ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1494.5±18.35µs    11.1 MB/sec     1.00  1493.1±20.33µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    169.6±2.49µs    17.4 MB/sec     1.01    171.0±2.51µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.03ms     8.0 MB/sec     1.00      3.2±0.02ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @charliermarsh on 2023-08-02 14:58_

---

_Merged by @charliermarsh on 2023-08-02 15:54_

---

_Closed by @charliermarsh on 2023-08-02 15:54_

---

_Branch deleted on 2023-08-02 15:54_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:862 on 2023-08-03 07:07_

Nit: This seems unrelated to this PR. I think it would have been good to split out the decorators handling from this otherwise pure refactor.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/arguments.rs`:127 on 2023-08-03 07:07_

This should not have been moved. It belongs to `ExprCall`

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_class_def.rs`:44 on 2023-08-03 07:10_

Nit: I find this name confusing. I would expect it to take an iterator as input (or something stateful that is after positioned after the trailing trivia). How about `after_trailing_trivia`? 

Related: There's a `token.kind().is_triva` method to avoid listing all trivia kinds. 

Related 2: I think the method should return 

---

_@MichaReiser reviewed on 2023-08-03 07:11_

---

_@MichaReiser reviewed on 2023-08-03 07:13_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_class_def.rs`:83 on 2023-08-03 07:13_

Nit: Add a `arguments.is_empty()` helper.

---

_@MichaReiser reviewed on 2023-08-03 07:14_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_class_def.rs`:61 on 2023-08-03 07:14_

Same: I think this change should have been a separate PR because it goes beyond the refactor.

---

_@MichaReiser reviewed on 2023-08-03 07:16_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_class_def.rs`:90 on 2023-08-03 07:16_

I think this is the first usage where another node formats a node's dangling comments. I also don't understand the reason for it from the comment above because all examples in the comment above have parentheses, but the point here seems to be to remove the parentheses. 

Why not move this into `Arguments`? It removes the need for querying the dangling comments here (we only have to do it in arguments) and is more in line of what we do elsewhere. 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1191 on 2023-08-03 07:33_

Nit: A more generalized implementation of this could be:

```rust
if comment.line_position().is_end_of_line() {
    let mut lexer = SimpleTokenizer::new(
        locator.contents(),
        TextRange::new(comment.enclosing_node().start(), comment.start()),
    )
    .skip_trivia()
    .skip_while(|t| {
        matches!(
            t.kind(),
            SimpleTokenKind::LParen | SimpleTokenKind::LBrace | SimpleTokenKind::LBracket
        )
    });

    if lexer.next().is_none() {
        return CommentPlacement::dangling(comment.enclosing_node(), comment);
    }
}

CommentPlacement::Default(comment)
```



---

_@MichaReiser reviewed on 2023-08-03 07:33_

---

_@charliermarsh reviewed on 2023-08-03 13:29_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/stmt_class_def.rs`:61 on 2023-08-03 13:29_

To me this is squarely part of the PR and the desired refactor.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/stmt_class_def.rs`:83 on 2023-08-03 13:30_

Good call, I believe this now exists on main but didn't at the time.

---

_@charliermarsh reviewed on 2023-08-03 13:30_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/stmt_class_def.rs`:44 on 2023-08-03 13:30_

(FWIW: all of this code already existed for functions and was not being applied to classes.)

---

_@charliermarsh reviewed on 2023-08-03 13:30_

---

_@charliermarsh reviewed on 2023-08-03 13:31_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:862 on 2023-08-03 13:31_

You're right that it'd be better as its own PR but I deemed it tolerable to include it here. All of this code already existed for functions and was missing from classes, so it was no net-new behavior.


---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/stmt_class_def.rs`:90 on 2023-08-03 13:33_

This is important: we need to handle the arguments dangling comments, but we don't want to render the arguments.

Given:

```python
class C( # foo
  ): pass
```

We want:
```python
class C:  # foo
  pass
```

We're _removing_ the arguments node entirely, and moving its comments onto the class identifier.


---

_@charliermarsh reviewed on 2023-08-03 13:33_

---

_@charliermarsh reviewed on 2023-08-03 13:35_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/stmt_class_def.rs`:90 on 2023-08-03 13:35_

I could handle it in placement instead of here?

---

_@MichaReiser reviewed on 2023-08-03 13:50_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_class_def.rs`:90 on 2023-08-03 13:50_

That might be easier to understand if it is easy enough

---

_Review comment by @zanieb on `crates/ruff_python_formatter/src/comments/placement.rs`:1191 on 2023-08-03 15:20_

cc @charliermarsh are we going to use this to consolidate the Arguements / TypeParams implementations?

---

_@zanieb reviewed on 2023-08-03 15:20_

---

_@charliermarsh reviewed on 2023-08-03 15:23_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1191 on 2023-08-03 15:23_

I can do another pass on this and consolidate.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/stmt_class_def.rs`:44 on 2023-08-03 16:11_

What was "Related 2" here?

---

_@charliermarsh reviewed on 2023-08-03 16:11_

---

_@charliermarsh reviewed on 2023-08-03 19:57_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1191 on 2023-08-03 19:57_

https://github.com/astral-sh/ruff/pull/6315

---

_@MichaReiser reviewed on 2023-08-03 20:07_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_class_def.rs`:44 on 2023-08-03 20:07_

Whoops. That I was unsure if it should return an option 

---

_@charliermarsh reviewed on 2023-08-03 20:09_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/stmt_class_def.rs`:44 on 2023-08-03 20:09_

:joy:

---
