```yaml
number: 6592
title: Format function and class definitions into a single line if its body is an ellipsis
type: pull_request
state: merged
author: tjkuson
labels:
  - formatter
assignees: []
merged: true
base: main
head: single-line-ellipsis
created_at: 2023-08-15T10:04:28Z
updated_at: 2023-08-29T10:44:07Z
url: https://github.com/astral-sh/ruff/pull/6592
synced_at: 2026-01-12T15:55:22Z
```

# Format function and class definitions into a single line if its body is an ellipsis

---

_@tjkuson_

## Summary

Formats

```python
class A:
    ...
```

to

```python
class A: ...
```

Closes #5822.

## Test Plan

`cargo test`


---

_Marked ready for review by @tjkuson on 2023-08-15 10:38_

---

_Comment by @github-actions[bot] on 2023-08-15 11:02_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.3±0.02ms    12.2 MB/sec    1.00      3.3±0.03ms    12.3 MB/sec
formatter/numpy/ctypeslib.py               1.00    671.3±7.06µs    24.8 MB/sec    1.00    674.7±6.34µs    24.7 MB/sec
formatter/numpy/globals.py                 1.00     69.1±0.39µs    42.7 MB/sec    1.00     69.3±0.43µs    42.6 MB/sec
formatter/pydantic/types.py                1.00  1310.0±13.27µs    19.5 MB/sec    1.01  1324.9±14.02µs    19.2 MB/sec
linter/all-rules/large/dataset.py          1.00     11.1±0.04ms     3.7 MB/sec    1.00     11.1±0.04ms     3.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.0±0.01ms     5.5 MB/sec    1.00      3.0±0.03ms     5.5 MB/sec
linter/all-rules/numpy/globals.py          1.03    337.1±1.20µs     8.8 MB/sec    1.00    326.5±1.11µs     9.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.02ms     4.4 MB/sec    1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.02      5.8±0.02ms     7.0 MB/sec    1.00      5.7±0.02ms     7.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1208.3±5.44µs    13.8 MB/sec    1.00   1194.0±8.99µs    13.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    124.7±0.44µs    23.7 MB/sec    1.01    125.4±5.41µs    23.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.6±0.01ms     9.9 MB/sec    1.00      2.5±0.01ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.02      4.6±0.28ms     8.8 MB/sec     1.00      4.5±0.20ms     9.0 MB/sec
formatter/numpy/ctypeslib.py               1.02   964.0±67.52µs    17.3 MB/sec     1.00   947.7±44.68µs    17.6 MB/sec
formatter/numpy/globals.py                 1.04     99.4±6.72µs    29.7 MB/sec     1.00     95.5±4.08µs    30.9 MB/sec
formatter/pydantic/types.py                1.01  1937.5±114.58µs    13.2 MB/sec    1.00  1917.2±156.22µs    13.3 MB/sec
linter/all-rules/large/dataset.py          1.00     16.5±0.49ms     2.5 MB/sec     1.01     16.6±0.45ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.5±0.20ms     3.7 MB/sec     1.00      4.5±0.18ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.02   569.9±39.92µs     5.2 MB/sec     1.00   560.9±37.18µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.02      8.6±0.33ms     3.0 MB/sec     1.00      8.5±0.34ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.0±0.30ms     4.5 MB/sec     1.02      9.2±0.36ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1912.2±80.18µs     8.7 MB/sec     1.08      2.1±0.13ms     8.1 MB/sec
linter/default-rules/numpy/globals.py      1.01   228.3±14.48µs    12.9 MB/sec     1.00   225.5±12.04µs    13.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.15ms     6.3 MB/sec     1.01      4.1±0.17ms     6.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser reviewed on 2023-08-15 16:57_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_class_def.rs`:143 on 2023-08-15 16:57_

I think we need to generalize this logic because black doesn't just apply this to classes and functions but to any compound statement (it seems). 


```python
while True: ...

for i in []: ...

if True: ...

with True: ...

match x:
    case 1: ...
```


We can can take a similar approach as I took in https://github.com/astral-sh/ruff/pull/6593/files#diff-2a7940151585697a7844053afc3fc5c7beafc616a9e1dc49ec4d43b98258dc35 with `clause_header` 

```rust
pub(crate) struct FormatClauseBody<'a> {
	body: &'a Suite
	kind: SuiteKind,
	trailing_comments: &'a [SourceComment],
}

impl FormatClauseBody {
	#[must_use]
	pub(crate) fn with_kind(mut self, kind: SuiteKind)  -> Self {
		self.kind = kind;
		self
	}
}

pub(crate) fn clause_body(body: &Suite, trailing_colon_comments: &'a [SourceComment]) -> FormatClauseBody {
	FormatClauseBody { body, kind: SuiteKind::default(), trailing_comments }
}

impl Format<PyFormatContext<'_>> or FormatClauseBody<'_> {
  fn fmt(&self, f: &mut Formatter<PyFormatContext<'ast>>) -> FormatResult<()> {
		if f.options().source_type().is_stub() && contains_only_an_ellipsis(self.body, f.context().comments()) && self.trailing_comments.is_empty(){
			self.body.format().with_options(self.kind).fmt(f)
		} else {
			self.block_indent(&body.format().with_options(self.kind)).fmt(f)
		}
  }
}
```

And I think you have to extend `contains_only_an_ellipsis` to assert that the `...` has no comments by using `f.context().comments().has_comments(stmt)` 

---

_@MichaReiser reviewed on 2023-08-15 16:58_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_class_def.rs`:143 on 2023-08-15 16:58_

Oh, it seems `has_comments` no longer exists on `Comments`. You can re-introduce it by adding the following method

```rust
#[inline]
    pub(crate) fn has_comments<T>(&self, node: T) -> bool
    where
        T: Into<AnyNodeRef<'a>>,
    {
        self.data
            .comments
            .has(&NodeRefEqualityKey::from_ref(node.into()))
    }
```

---

_Label `formatter` added by @charliermarsh on 2023-08-16 17:37_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/top_level.pyi`:82 on 2023-08-17 06:48_

Could you add an example with a leading comment too

```
class EllipsisWithLeadingComment:
	# leading
	... 
```

and maybe another with two trailing comments

```
class EllispsisWithMultipleTrailing: # trailing class comment
	... # trailing ellipsis comment
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:347 on 2023-08-17 06:52_

I think I was wrong with suggesting `has_comments`. It should be sufficient to these whether `stmt` has any leading comments. Let's move the `has_comments check after the match. Looking up comments is more expensive than testing if it is a const ellipsis expression

```suggestion
        [Stmt::Expr(stmt @ ast::StmtExpr { value, .. })] => {
            matches!(
                value.as_ref(),
                Expr::Constant(ast::ExprConstant {
                    value: Constant::Ellipsis,
                    ..
                })
            ) && !comments.has_leading_comments((stmt)
        }
```


---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:518 on 2023-08-17 06:52_

Nit: Can we move this to `caluse.rs`?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:480 on 2023-08-17 06:53_

We'll need a method to set the `kind` so that `class` and `functions` can set the right suite kind.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_class_def.rs`:136 on 2023-08-17 06:55_

It may have been because I incorrectly suggested to use `has_comments` and we'll need a way to pass the `SuiteKind::Class` to `clause_body`, e.g. by adding a `with_suite_kind` method to it

---

_@MichaReiser requested changes on 2023-08-17 06:57_

This starts to looking good. I left a few comments that hopefully help to solve the TODOs. We have to use the new `clause_body` everywhere where we do `block_indent(&body.format())` today (all compound statements). But we can do this once the class and function formatting is stable.

---

_@tjkuson reviewed on 2023-08-18 11:39_

---

_Review comment by @tjkuson on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/top_level.pyi`:82 on 2023-08-18 11:39_

Black fails to format that first example. What should ruff do?

---

_Converted to draft by @tjkuson on 2023-08-18 11:42_

---

_@MichaReiser reviewed on 2023-08-18 11:59_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/top_level.pyi`:82 on 2023-08-18 11:59_

We should keep the first example as is (not collapse the `...`). 

---

_Comment by @MichaReiser on 2023-08-18 12:28_

Wow, nice work. This shoots our typeshed compatibility up by more than 20%



project | similarity index before | after
-- | -- | --
cpython | 0.75473 | 0.75473
django | 0.99805 | 0.99805
transformers | 0.99620 | 0.99620
twine | 0.99876 | 0.99876
typeshed | 0.74292 | 0.99953
warehouse | 0.99601 | 0.99601
zulip | 0.99727 | 0.99727



---

_Comment by @tjkuson on 2023-08-18 12:37_

I'm trying to work out why it formats

```python
class EllipsisWithLeadingComment:
	# leading
	... 
```

to

```python
class EllipsisWithLeadingComment: # leading
...
```

Once that's sorted, it should be possible to do the same for every other clause body...


---

_Comment by @MichaReiser on 2023-08-18 12:54_

> I'm trying to work out why it formats
> 
> ```python
> class EllipsisWithLeadingComment:
> 	# leading
> 	... 
> ```
> 
> to
> 
> ```python
> class EllipsisWithLeadingComment: # leading
> ...
> ```
> 
> Once that's sorted, it should be possible to do the same for every other clause body...

Sounds good. Let me know if you want any help. 

---

_Marked ready for review by @tjkuson on 2023-08-18 13:39_

---

_Review requested from @MichaReiser by @tjkuson on 2023-08-18 13:39_

---

_Comment by @tjkuson on 2023-08-18 15:02_

The logic for match statements seems different to other compound statements

```rust
let mut last_case = first;

for case in cases_iter {
    write!(
        f,
        [block_indent(&format_args!(
            leading_alternate_branch_comments(
                comments.leading(case),
                last_case.body.last(),
            ),
            case.format()
        ))]
    )?;
    last_case = case;
}
```

I'm not quite sure how this would with `clause_body`.

---

_@MichaReiser approved on 2023-08-18 15:12_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_while.rs`:46 on 2023-08-18 15:16_

Do we need to change the formatting for the else case too (line 66):

```
while True:
	pass
else:
	...
````

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_if.rs`:39 on 2023-08-18 15:17_

We'll need to use `clause_body` in the `else` and `elif` cases too (line 101). Could we add test cases as well.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_for.rs`:67 on 2023-08-18 15:18_

We'll also need to change the `else` case on line 88.

---

_Comment by @MichaReiser on 2023-08-18 15:19_

Nice, this change is so cool. Thanks for working on it.

Do we need to change the try formatting as well?

> The logic for match statements seems different to other compound statements

I understand this isn't needed on the `match` level (that renders the cases), only on the case level. You can find its implementation here

https://github.com/astral-sh/ruff/blob/0cea4975fcfe09716c084c0a18d1b4c4cd9b8f05/crates/ruff_python_formatter/src/other/match_case.rs#L27-L63



---

_@MichaReiser reviewed on 2023-08-18 15:19_

---

_Comment by @MichaReiser on 2023-08-18 16:31_

I think the last thing now is to add the handling to the try statement

https://github.com/astral-sh/ruff/blob/0cea4975fcfe09716c084c0a18d1b4c4cd9b8f05/crates/ruff_python_formatter/src/statement/stmt_try.rs#L136-L143

---

_Merged by @MichaReiser on 2023-08-21 07:02_

---

_Closed by @MichaReiser on 2023-08-21 07:02_

---

_Branch deleted on 2023-08-29 10:44_

---
