```yaml
number: 6410
title: Group function definition parameters with return type annotations
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/function-signature-ii
created_at: 2023-08-08T01:37:17Z
updated_at: 2023-08-09T12:38:11Z
url: https://github.com/astral-sh/ruff/pull/6410
synced_at: 2026-01-12T02:52:04Z
```

# Group function definition parameters with return type annotations

---

_Pull request opened by @charliermarsh on 2023-08-08 01:37_

## Summary

This PR removes the group around function definition parameters, instead grouping the parameters with the type parameters and return type annotation.

This increases Zulip's similarity score from 0.99385 to 0.99699, so it's a meaningful improvement. However, there's at least one stability error that I'm working on, and I'm really just looking for high-level feedback at this point, because I'm not happy with the solution.

Closes https://github.com/astral-sh/ruff/issues/6352.

## Test Plan

Before:

- `zulip`: 0.99396
- `django`: 0.99784
- `warehouse`: 0.99578
- `build`: 0.75436
- `transformers`: 0.99407
- `cpython`: 0.75987
- `typeshed`: 0.74432

After:

- `zulip`: 0.99702
- `django`: 0.99784
- `warehouse`: 0.99585
- `build`: 0.75623
- `transformers`: 0.99470
- `cpython`: 0.75988
- `typeshed`: 0.74853


---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-08 01:37_

---

_Review requested from @konstin by @charliermarsh on 2023-08-08 01:37_

---

_@charliermarsh reviewed on 2023-08-08 01:38_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/parentheses.rs`:157 on 2023-08-08 01:38_

In short: we need to avoid this `group` call in _certain_ uses of `parenthesized`. I could add an entirely separate builder if we prefer, but I'll need to add a separately builder for `empty_parenthesized` too.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/other/parameters.rs`:251 on 2023-08-08 01:38_

E.g., here is where we're ungrouping.

---

_@charliermarsh reviewed on 2023-08-08 01:38_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/parentheses.rs`:157 on 2023-08-08 01:39_

(For `empty_parenthesized`, we could even remove the `group` entirely and require the callers to wrap in `group`. For `parenthesized`, we can't do that, because we wrap the `group` in a `fits_expanded` in the `NodeLevel::Expression` case.)

---

_@charliermarsh reviewed on 2023-08-08 01:39_

---

_Label `formatter` added by @charliermarsh on 2023-08-08 01:39_

---

_Converted to draft by @charliermarsh on 2023-08-08 01:40_

---

_Marked ready for review by @charliermarsh on 2023-08-08 01:40_

---

_Comment by @charliermarsh on 2023-08-08 01:40_

As I said in the summary, I'd like feedback on `with_ungroup`, or whether I should be doing this completely differently. I'm not yet looking for approval (but I also don't want to mark it as draft, since I do want eyes on it).

---

_Comment by @charliermarsh on 2023-08-08 02:07_

(I think this also breaks the behavior whereby we insert a trailing comma if the parameters group breaks, but I believe I can fix that.)

---

_Comment by @github-actions[bot] on 2023-08-08 02:25_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      8.4±0.05ms     4.9 MB/sec    1.00      8.2±0.03ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.03  1637.1±13.26µs    10.2 MB/sec    1.00  1592.1±10.64µs    10.5 MB/sec
formatter/numpy/globals.py                 1.03    176.0±0.55µs    16.8 MB/sec    1.00    170.4±1.99µs    17.3 MB/sec
formatter/pydantic/types.py                1.03      3.6±0.05ms     7.1 MB/sec    1.00      3.5±0.02ms     7.3 MB/sec
linter/all-rules/large/dataset.py          1.00     10.5±0.02ms     3.9 MB/sec    1.01     10.6±0.03ms     3.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.01ms     5.8 MB/sec    1.00      2.9±0.01ms     5.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    319.0±2.29µs     9.3 MB/sec    1.00    316.8±1.66µs     9.3 MB/sec
linter/all-rules/pydantic/types.py         1.12      5.5±0.09ms     4.7 MB/sec    1.00      4.9±0.03ms     5.2 MB/sec
linter/default-rules/large/dataset.py      1.01      5.5±0.02ms     7.4 MB/sec    1.00      5.5±0.02ms     7.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1153.3±5.46µs    14.4 MB/sec    1.00   1127.2±5.47µs    14.8 MB/sec
linter/default-rules/numpy/globals.py      1.02    117.3±2.69µs    25.2 MB/sec    1.00    114.6±0.30µs    25.7 MB/sec
linter/default-rules/pydantic/types.py     1.02      2.5±0.01ms    10.3 MB/sec    1.00      2.4±0.02ms    10.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03     10.0±0.12ms     4.1 MB/sec    1.00      9.8±0.03ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.06  1963.6±38.15µs     8.5 MB/sec    1.00  1860.4±15.01µs     9.0 MB/sec
formatter/numpy/globals.py                 1.02    197.5±1.86µs    14.9 MB/sec    1.00    194.0±3.57µs    15.2 MB/sec
formatter/pydantic/types.py                1.04      4.3±0.08ms     5.9 MB/sec    1.00      4.2±0.04ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.02     13.2±0.22ms     3.1 MB/sec    1.00     12.9±0.18ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.03ms     4.6 MB/sec    1.00      3.6±0.03ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.02    374.9±7.97µs     7.9 MB/sec    1.00    367.6±5.51µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.13      6.9±0.05ms     3.7 MB/sec    1.00      6.1±0.05ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.0±0.18ms     5.8 MB/sec    1.00      6.9±0.12ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1404.7±26.09µs    11.9 MB/sec    1.00  1371.8±18.16µs    12.1 MB/sec
linter/default-rules/numpy/globals.py      1.03    143.6±1.47µs    20.5 MB/sec    1.00    138.8±1.01µs    21.3 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.1±0.01ms     8.3 MB/sec    1.00      3.0±0.02ms     8.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/parentheses.rs`:190 on 2023-08-08 07:06_

I would expect that content with `ungroup = true` never gets grouped. Meaning, we should only use `fits_expanded(&group(&inner))` is false.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_function_def.rs`:60 on 2023-08-08 07:08_

Is it necessary for the type params to be part of the group too? If so, can you add tests that show how the expansion for a function with type params, parameters, and return type looks like

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/parentheses.rs`:159 on 2023-08-08 07:16_

We can unify these two branches. `dangling_open_parenthesis_comments` renders nothing if `comments` is empty. 

I'm leaning towards inlining these five lines in `parameters.rs` rather than changing the grouping here.

```
	let mut f = WithNodeLevel::new(NodeLevel::ParenthesizedExpression, f);
    write!(f, [ 
	    	text(self.left),
       	&dangling_open_parenthesis_comments(self.comments),
    		&soft_block_indent(&Arguments::from(&self.content)),
      		text(self.right)
      ]
	)
```

We aren't testing through the case of using `ungroup` inside another parenthesized expression (can never apply to arguments) and this seems to be a one-off problem. 

We can introduce a new builder if you're concerned about code-reuse.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/parentheses.rs`:383 on 2023-08-08 07:17_

Nit: `without_group`? 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/parameters.rs`:251 on 2023-08-08 07:29_

Is this ungrouping necessary for the empty parenthesized case? I believe the extra group is unnecessary because a trailing comment will always make all parent groups extend. In addition, using a group for empty parenthesized may always be incorrect because we never want the printer to break right after the `(` 

```python
a = bbbbbbbbbbbbbbbbbbbbb + ()
                                                             ^ max line width

# We don't want
a = bbbbbbbbbbbbbbbbbbbbb + (
)
```



```rust

let (end_of_line_comments, own_line_comments) = self.comments.split_at(end_of_line_split);

write!(
	f, 
	[
		text(self.left),
		trailing_comments(end_of_line_comments),
	]
)?;

if !own_line_comments.is_empty() {
	block_indent(&dangling_comments(own_line_comments)).fmt(f)?;
} else if !end_of_line_comments.is_empty() {
	hard_line_break.fmt(f)?;
}
		
text(self.right).fmt(f)
```

---

_@MichaReiser reviewed on 2023-08-08 07:31_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/parentheses.rs`:118 on 2023-08-08 10:00_

nit: `no_group` instead of `ungroup`

---

_@konstin approved on 2023-08-08 10:09_

Looking at the black test cases, i think the following is another one of those where black doesn't add parentheses if it doesn't help with fitting:

black:
```black
def foo(
    a: int,
    b: int,
    c: int,
) -> intsdfsafafafdfdsasdfsfsdfasdfafdsafdfdsfasdskdsdsfdsafdsafsdfdasfffsfdsfdsafafhdskfhdsfjdslkfdlfsdkjhsdfjkdshfkljds:
    return 2
```
ours:
```python
def foo(
    a: int,
    b: int,
    c: int,
) -> (
    intsdfsafafafdfdsasdfsfsdfasdfafdsafdfdsfasdskdsdsfdsafdsafsdfdasfffsfdsfdsafafhdskfhdsfjdslkfdlfsdkjhsdfjkdshfkljds
):
    return 2
```

Otherwise black_compatibility@simple_cases__return_annotation_brackets.py.snap looks good

---

_Comment by @charliermarsh on 2023-08-08 20:48_

Okay, this got _way_ simpler thanks for the feedback.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-08 20:48_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/parameters.rs`:258 on 2023-08-09 06:55_

```suggestion
                    dangling_open_parenthesis_comments(parenthesis_dangling),
                    soft_block_indent(&group(&format_inner)),
```

---

_@MichaReiser approved on 2023-08-09 06:56_

---

_Comment by @MichaReiser on 2023-08-09 07:35_

Current dependencies on/for this PR:
* main
  * **PR #6410** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/6410" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a>  👈
    * **PR #6439** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/6439" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/6410?utm_source=stack-comment).

---

_Merged by @charliermarsh on 2023-08-09 12:13_

---

_Closed by @charliermarsh on 2023-08-09 12:13_

---

_Branch deleted on 2023-08-09 12:14_

---
