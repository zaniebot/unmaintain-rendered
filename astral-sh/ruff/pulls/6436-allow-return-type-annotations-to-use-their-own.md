```yaml
number: 6436
title: Allow return type annotations to use their own parentheses
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/function-returns-wrap
created_at: 2023-08-09T04:15:05Z
updated_at: 2023-08-11T18:42:49Z
url: https://github.com/astral-sh/ruff/pull/6436
synced_at: 2026-01-12T02:52:04Z
```

# Allow return type annotations to use their own parentheses

---

_Pull request opened by @charliermarsh on 2023-08-09 04:15_

## Summary

This PR modifies our logic for wrapping return type annotations. Previously, we _always_ wrapped the annotation in parentheses if it expanded; however, Black only exhibits this behavior when the function parameters is empty (i.e., it doesn't and can't break). In other cases, it uses the normal parenthesization rules, allowing nodes to bring their own parentheses.

For example, given:

```python
def xxxxxxxxxxxxxxxxxxxxxxxxxxxx() -> Set[
    "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
]:
    ...

def xxxxxxxxxxxxxxxxxxxxxxxxxxxx(x) -> Set[
    "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
]:
    ...
```

Black will format as:

```python
def xxxxxxxxxxxxxxxxxxxxxxxxxxxx() -> (
    Set[
        "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    ]
):
    ...


def xxxxxxxxxxxxxxxxxxxxxxxxxxxx(
    x,
) -> Set[
    "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
]:
    ...
```

Whereas, prior to this PR, Ruff would format as:

```python
def xxxxxxxxxxxxxxxxxxxxxxxxxxxx() -> (
    Set[
        "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    ]
):
    ...


def xxxxxxxxxxxxxxxxxxxxxxxxxxxx(
    x,
) -> (
    Set[
        "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    ]
):
    ...
```

Closes https://github.com/astral-sh/ruff/issues/6431.

## Test Plan

Before:

- `zulip`: 0.99702
- `django`: 0.99784
- `warehouse`: 0.99585
- `build`: 0.75623
- `transformers`: 0.99470
- `cpython`: 0.75988
- `typeshed`: 0.74853

After:

- `zulip`: 0.99724
- `django`: 0.99791
- `warehouse`: 0.99586
- `build`: 0.75623
- `transformers`: 0.99474
- `cpython`: 0.75956
- `typeshed`: 0.74857



---

_Converted to draft by @charliermarsh on 2023-08-09 04:16_

---

_Label `formatter` added by @charliermarsh on 2023-08-09 04:16_

---

_Comment by @charliermarsh on 2023-08-09 04:19_

This seems to do the right thing, but I'm already anticipating that I'm misunderstanding something around why / how Black makes it decisions, and the relationship to our grouping and parenthesization logic.

---

_Marked ready for review by @charliermarsh on 2023-08-09 04:26_

---

_Comment by @charliermarsh on 2023-08-09 04:26_

(Looks like I have one failing stability test.)

---

_Converted to draft by @charliermarsh on 2023-08-09 04:30_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/stmt_function_def.rs`:72 on 2023-08-09 04:43_

This is meant to capture "Can the parameters break?". Like, semantically, it'd be nice if I could add a group around parameters and check if it breaks, but I don't think I can do that.

---

_@charliermarsh reviewed on 2023-08-09 04:43_

---

_@charliermarsh reviewed on 2023-08-09 04:44_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/parentheses.rs`:57 on 2023-08-09 04:44_

I'm not sure how conservative we want to be in adding these.

---

_Comment by @github-actions[bot] on 2023-08-09 05:25_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.6±0.33ms     4.2 MB/sec    1.00      9.6±0.30ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.01  1876.6±62.75µs     8.9 MB/sec    1.00  1850.8±78.02µs     9.0 MB/sec
formatter/numpy/globals.py                 1.02   218.5±12.49µs    13.5 MB/sec    1.00    213.3±8.63µs    13.8 MB/sec
formatter/pydantic/types.py                1.02      4.1±0.19ms     6.3 MB/sec    1.00      4.0±0.14ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     12.5±0.29ms     3.3 MB/sec    1.02     12.7±0.28ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.12ms     4.9 MB/sec    1.00      3.3±0.09ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   479.2±14.32µs     6.2 MB/sec    1.02   487.2±17.72µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.16ms     3.9 MB/sec    1.02      6.6±0.17ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.14ms     6.2 MB/sec    1.04      6.8±0.18ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1426.6±34.31µs    11.7 MB/sec    1.03  1466.6±67.49µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.02    179.9±7.27µs    16.4 MB/sec    1.00    176.2±5.33µs    16.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.06ms     8.6 MB/sec    1.01      3.0±0.07ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00     11.6±0.64ms     3.5 MB/sec     1.03     12.0±0.72ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.13ms     8.1 MB/sec     1.14      2.3±0.09ms     7.1 MB/sec
formatter/numpy/globals.py                 1.00   242.7±28.57µs    12.2 MB/sec     1.12   270.9±26.35µs    10.9 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.28ms     5.8 MB/sec     1.12      4.9±0.20ms     5.2 MB/sec
linter/all-rules/large/dataset.py          1.01     15.6±0.59ms     2.6 MB/sec     1.00     15.4±0.41ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.18ms     3.9 MB/sec     1.00      4.2±0.19ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.06   568.0±26.72µs     5.2 MB/sec     1.00   537.2±22.34µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.0±0.32ms     3.2 MB/sec     1.01      8.1±0.32ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.10      9.5±0.63ms     4.3 MB/sec     1.00      8.6±0.29ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1758.3±125.96µs     9.5 MB/sec    1.00  1761.2±62.14µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00   197.6±14.45µs    14.9 MB/sec     1.16   229.6±10.22µs    12.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.18ms     7.3 MB/sec     1.10      3.8±0.20ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/parentheses.rs`:57 on 2023-08-09 05:59_

Could we use `parenthesize_if_expands` in `StmtFunctionDef` directly, as it seems to be a one-off requirement?

Edit: The difference is that `maybe_parenthesize` respects `NeedsParentheses`. The *ignoring the expression's parenthesization requirements* is misleading because the point over `parenthesize_if_expands` is precisely that it respects `NeedsParentheses` when it returns `Always`. How about naming this variant `IfBreaksOrIfRequired` (I'm not super happy with it, because `IfRequired` is intended to be a subset of `IfBreaks`) or `IfBreaksIncludingNever`

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_function_def.rs`:72 on 2023-08-09 06:01_

You may be able to inspect the IR if you want. But I prefer AST based rules because they're easier understood

```rust
let mut recording = f.start_recording();

write!(recording, [item.parameters.format()])?;

let recorded = recording.stop();
// now inspect the document
recorded.iter().any(|element| ....)
```

---

_@MichaReiser reviewed on 2023-08-09 06:39_

This is interesting and I wonder how intentional the behavior is. I tried to deduce the behavior from Black's source code. I  can confirm that the behavior is different if the function has no arguments, but I haven't yet found the place where the branching happens:

* Black uses `left_hand_split` for function definitions (rather than the usual `rhs`) [[source](https://github.com/psf/black/blob/01b8d3d4095ebdb91d0d39012a517931625c63cb/src/black/linegen.py#L547-L548)]
* For empty: It forces the optional parentheses of the return type and applies the default transforms for parenthesized expressions (`delimiter_split`, `standalone_comment_split`, `rhs`). This matches `optional_parentheses` 
* Non empty: It splits after the `(` of the arguments and then applies the default transform for *unparenthesized* expressions (`rhs`, `hug_power_op`). This matches `mabye_parenthesize` with `Parenthesize::IfBreaks`


---

_@MichaReiser reviewed on 2023-08-09 06:45_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:205 on 2023-08-09 06:45_

Is the path `OptionalParentheses::Multiline` ever taken? Doesn't it already pass the aboth multiline case (because `parenthesize != Parenthesize::IfRequired` is true

---

_@MichaReiser reviewed on 2023-08-09 06:56_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_function_def.rs`:68 on 2023-08-09 06:56_

This is a nice win :) 

---

_@charliermarsh reviewed on 2023-08-09 20:53_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/mod.rs`:205 on 2023-08-09 20:53_

Good call.

---

_@charliermarsh reviewed on 2023-08-09 20:54_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/parentheses.rs`:57 on 2023-08-09 20:54_

Tried to clarify, thank you...

---

_Comment by @charliermarsh on 2023-08-09 20:54_

(I need to figure out the instability here, then will re-request a review.)

---

_Comment by @charliermarsh on 2023-08-09 23:41_

Having trouble with this, looking for help.

Given:
```python
def double(a: int) -> (
    int # Hello
):
    return 2*a
```

We first format as:
```python
def double(
    a: int
) -> int:  # Hello
    return 2 * a
```

This is because we format `# Hello` as a trailing end-of-line comment on the return annotation, which leads to an `expand_parent()`, since we always expand parents for trailing comments. But the expanded parent group is the entire parameters _and_ return type, so the parameters breaks in that odd way, I guess?

When we reformat, we get:
```python
def double(a: int) -> int:  # Hello
    return 2 * a
```

Since the `# Hello` gets placed as a dangling comment on the function definition via `placement.rs`.

Is there any way for me to avoid the breaking in the first format? (This _does_ get fixed by https://github.com/astral-sh/ruff/pull/6464 since we don't incorrectly break, but perhaps we won't end up merging that.)


---

_Marked ready for review by @charliermarsh on 2023-08-10 00:27_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:164 on 2023-08-10 06:09_

You want stacked diffs. I can't approve this PR now because I disagree with this change ;)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:204 on 2023-08-10 06:11_

I think you still want an extra case for `Multiline` and `IfBreaksOrRequired` because parentheses are now omitted if `can_omit_optional_parentheses` returns `True`. Is is this what we want? Can we come up with a test case that expands over multiple lines and `can_omit_optional_parentheses` returns `True`.

---

_@MichaReiser requested changes on 2023-08-10 06:12_

Let's remove the comment formatting changes.

---

_@charliermarsh reviewed on 2023-08-10 12:42_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/format.rs`:164 on 2023-08-10 12:42_

Sorry, the commits are correct, I just forgot to set the base branch.

---

_Comment by @charliermarsh on 2023-08-10 12:43_

We can certainly remove the comment formatting changes, but it then requires coming up with a strategy for resolving the instability that I documented above, which either requires further deviating from Black or changing the groups in a way that I don’t fully understand.

---

_Comment by @MichaReiser on 2023-08-10 13:52_

One option would be to group the `parameters` to revert to right to left breaking if we know that the right side has a trailing comment (it is guaranteed to break). Meaning, we then use our previous logic where we first expand the return type (which is guaranteed to expand because of the comment), and only then the parameters.

```rust
            let group_function_parameters = item
                .returns
                .as_deref()
                .is_some_and(|return_type| comments.has_trailing_comments(return_type));

            if group_function_parameters {
                write!(f, [group(&item.parameters.format())])?;
            } else {
                write!(f, [item.parameters.format()])?;
            }
```

---

_Comment by @MichaReiser on 2023-08-10 13:57_

Never, this doesn't work (for the opposite reason):

```python
def xxxxxxxxxxxxxxxxxxxxxxxxxxxx(
    x, b, c, d
) -> (Set[
    "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
] # test
): 
    ...

# Pass 1
def xxxxxxxxxxxxxxxxxxxxxxxxxxxx(x, b, c, d) -> Set[
    "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
]:  # test
    ...

# Pass 2
def xxxxxxxxxxxxxxxxxxxxxxxxxxxx(
    x, b, c, d
) -> Set[
    "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
]:  # test
    ...

```

---

_Comment by @MichaReiser on 2023-08-10 14:27_

The easiest might be to group parameters if the return type doesn't have its own breakpoint and has a trailing comment. The list should be rather short because we know that we're in a return-type position. Rome has a similar function

https://github.com/rome/tools/blob/2655264565608e29229490101f7abbfa89a35598/crates/rome_js_formatter/src/js/declarations/function_declaration.rs#L258-L297

---

_Comment by @charliermarsh on 2023-08-10 15:45_

(Trying this.)

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-10 16:48_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/stmt_function_def.rs`:198 on 2023-08-10 16:51_

Hmm, no, this needs to be more expansive.

---

_@charliermarsh reviewed on 2023-08-10 16:51_

---

_Comment by @konstin on 2023-08-10 16:59_

Have you considered changing the definition of `IfRequired` instead?

From a quick search, we use `IfRequired` for await, yield/yield from, and (async) with. We format
```python
with (x := 1):
    pass

def g():
    yield (x := 1)

async def f():
    await (x := 1)
```
as
```python
with (x := 1):
    pass


def g():
    yield x := 1


async def f():
    await x := 1
```
where `yield x := 1` and `await x := 1` are syntax errors. (Black keeps all the parentheses and doesn't introduce a syntax error)

---

_@charliermarsh reviewed on 2023-08-10 17:00_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/stmt_function_def.rs`:198 on 2023-08-10 17:00_

I guess this will work for most real cases, but it's still unstable for code like:
```python
def double(a: int) -> (
    [int] # Hello
):
    return 2*a
```

---

_Comment by @charliermarsh on 2023-08-10 17:01_

I'll take a look! I'm first trying to figure out how to fix this instability.

---

_Converted to draft by @charliermarsh on 2023-08-11 04:47_

---

_Comment by @charliermarsh on 2023-08-11 05:09_

As a last resort, I suppose I could make this a dangling comment on the function, so that it always gets formatted after the `:` consistently. It would not be compatible with Black in all cases, but it would at least be stable, simple, and... _reasonable_.

---

_Comment by @charliermarsh on 2023-08-11 05:13_

Another deviation is that I could decide to _preserve_ the parentheses if a trailing comment is present there, e.g., make this stable formatting:

```python
def double(
    a: int
) -> (
    int  # Hello
):
    pass
```

(I haven't looked into how difficult this would be, but I assume it would be easier than what I'm trying to do here.)

---

_@MichaReiser reviewed on 2023-08-11 07:18_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/function.py`:438 on 2023-08-11 07:18_

This file is becoming massive. You may want to move some tests into a new file

---

_Comment by @MichaReiser on 2023-08-11 07:19_

Can you push your latest change so that I can play with them myself? 

---

_Comment by @charliermarsh on 2023-08-11 13:16_

I just pushed. If simpler, you can also remove the `should_group_parameters` to get the original behavior before that suggestion:

```diff
diff --git a/crates/ruff_python_formatter/src/statement/stmt_function_def.rs b/crates/ruff_python_formatter/src/statement/stmt_function_def.rs
index d47825cc2..39025b11c 100644
--- a/crates/ruff_python_formatter/src/statement/stmt_function_def.rs
+++ b/crates/ruff_python_formatter/src/statement/stmt_function_def.rs
@@ -60,15 +60,7 @@ impl FormatNodeRule<StmtFunctionDef> for FormatStmtFunctionDef {
         }
 
         let format_inner = format_with(|f: &mut PyFormatter| {
-            if should_group_function_parameters(
-                &item.parameters,
-                item.returns.as_deref(),
-                f.context(),
-            ) {
-                write!(f, [group(&item.parameters.format())])?;
-            } else {
-                write!(f, [item.parameters.format()])?;
-            }
+            write!(f, [item.parameters.format()])?;
 
             if let Some(return_annotation) = item.returns.as_ref() {
                 write!(f, [space(), text("->"), space()])?;
```

---

_Comment by @charliermarsh on 2023-08-11 16:41_

The current version deviates from Black, but it's at least stable... Specifically, we now preserve formatting like:

```python
def double(
    a: int
) -> (
    int  # Hello
):
    pass
```

I think this is _reasonable_ but perhaps not ideal.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/mod.rs`:204 on 2023-08-11 16:45_

You're right, good call.

---

_@charliermarsh reviewed on 2023-08-11 16:45_

---

_@charliermarsh reviewed on 2023-08-11 16:53_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/mod.rs`:191 on 2023-08-11 16:53_

This is a bit less DRY but I find it much easier to reason about the match when every `OptionalParentheses` has a single branch.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-11 16:53_

---

_Marked ready for review by @charliermarsh on 2023-08-11 16:53_

---

_Comment by @charliermarsh on 2023-08-11 16:59_

I think this formatting is arguably more consistent for us, since we format this:
```python
def double(
    a # comment
):
    return 2 * a
```

As:

```python
def double(
    a,  # comment
):
    return 2 * a
```

That is, we don't push those out to after the function definition (unlike Black).


---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_function_def.rs`:73 on 2023-08-11 17:50_

Nit: Would it be enough to test for leading and trailing comments only?



---

_@MichaReiser approved on 2023-08-11 17:51_

I like the outcome. It even preserves the authors comment choice:

e.g. I understand that we would keep

```python
def test() -> (
	[a, b ]  # noqa: Ruf01
): pass
```

---

_Merged by @charliermarsh on 2023-08-11 18:19_

---

_Closed by @charliermarsh on 2023-08-11 18:19_

---

_Branch deleted on 2023-08-11 18:19_

---
