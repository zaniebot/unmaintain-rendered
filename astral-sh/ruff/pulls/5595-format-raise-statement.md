```yaml
number: 5595
title: "Format `raise` statement"
type: pull_request
state: merged
author: LouisDISPA
labels:
  - formatter
assignees: []
merged: true
base: main
head: main
created_at: 2023-07-07T16:01:52Z
updated_at: 2023-07-10T19:23:50Z
url: https://github.com/astral-sh/ruff/pull/5595
synced_at: 2026-01-12T03:36:55Z
```

# Format `raise` statement

---

_Pull request opened by @LouisDISPA on 2023-07-07 16:01_

## Summary

This PR implements the formatting of `raise` statements. I haven't looked at the black implementation, this is inspired from from the `return` statements formatting.

## Test Plan

The black differences with insta.

I also compared manually some edge cases with very long string and call chaining and it seems to do the same formatting as black.

There is one issue:
```python
# input

raise OsError(
    "aksjdhflsakhdflkjsadlfajkslhfdkjsaldajlahflashdfljahlfksajlhfajfjfsaahflakjslhdfkjalhdskjfa"
) from a.aaaaa(aksjdhflsakhdflkjsadlfajkslhfdkjsaldajlahflashdfljahlfksajlhfajfjfsaahflakjslhdfkjalhdskjfa).a(aaaa)


# black

raise OsError(
    "aksjdhflsakhdflkjsadlfajkslhfdkjsaldajlahflashdfljahlfksajlhfajfjfsaahflakjslhdfkjalhdskjfa"
) from a.aaaaa(
    aksjdhflsakhdflkjsadlfajkslhfdkjsaldajlahflashdfljahlfksajlhfajfjfsaahflakjslhdfkjalhdskjfa
).a(
    aaaa
)


# ruff

raise OsError(
    "aksjdhflsakhdflkjsadlfajkslhfdkjsaldajlahflashdfljahlfksajlhfajfjfsaahflakjslhdfkjalhdskjfa"
) from a.aaaaa(
    aksjdhflsakhdflkjsadlfajkslhfdkjsaldajlahflashdfljahlfksajlhfajfjfsaahflakjslhdfkjalhdskjfa
).a(aaaa)
```

But I'm not sure this diff is the raise formatting implementation.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_raise.rs`:24 on 2023-07-07 16:31_

I think we'll need `Parenthesize::IfBreaks` here to support

```python
raise aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbb + cccccccccccccccccccccc + ddddddddddddddddddddddddd
```

(Yes, this is a very made up example)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_raise.rs`:35 on 2023-07-07 16:31_

Potentially use `Parenthesize::IfBreaks` here too

---

_@MichaReiser reviewed on 2023-07-07 16:32_

Yeah, you can ignore that issue. This is related to expression formatting. 

It would be great if you could add a few more tests with comments in different positions to see that this doesn't cause formatting errors:

```
raise (
	# comment
	a
) from (
	b
	# comment
)
```

---

_Comment by @github-actions[bot] on 2023-07-07 16:37_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.4±0.04ms     4.8 MB/sec    1.01      8.5±0.08ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   1797.8±3.34µs     9.3 MB/sec    1.03   1852.1±3.65µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    197.9±0.63µs    14.9 MB/sec    1.02    201.4±0.20µs    14.6 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.01ms     6.2 MB/sec    1.01      4.1±0.01ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.00     14.1±0.06ms     2.9 MB/sec    1.00     14.2±0.06ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.02ms     4.8 MB/sec    1.01      3.5±0.03ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    361.6±0.90µs     8.2 MB/sec    1.01    363.6±1.86µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.03ms     4.1 MB/sec    1.01      6.2±0.03ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.03ms     5.7 MB/sec    1.00      7.2±0.04ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1461.7±1.87µs    11.4 MB/sec    1.00  1465.3±12.86µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    155.6±0.26µs    19.0 MB/sec    1.00    155.9±0.27µs    18.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.1 MB/sec    1.01      3.2±0.01ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.6±0.12ms     4.2 MB/sec    1.03     10.0±0.18ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.07ms     7.9 MB/sec    1.05      2.2±0.04ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00    234.2±7.15µs    12.6 MB/sec    1.05    245.3±7.10µs    12.0 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.14ms     5.4 MB/sec    1.03      4.8±0.06ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.8±0.24ms     2.6 MB/sec    1.00     15.9±0.40ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.09ms     4.0 MB/sec    1.01      4.2±0.18ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    503.9±9.88µs     5.9 MB/sec    1.00    505.4±8.34µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.1±0.14ms     3.6 MB/sec    1.00      7.0±0.16ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.1±0.12ms     5.0 MB/sec    1.00      8.0±0.05ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1727.0±48.36µs     9.6 MB/sec    1.00  1716.8±33.80µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    200.2±3.69µs    14.7 MB/sec    1.03    206.2±6.99µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.10ms     7.0 MB/sec    1.00      3.6±0.09ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `formatter` added by @konstin on 2023-07-07 19:32_

---

_@LouisDISPA reviewed on 2023-07-07 22:03_

---

_Review comment by @LouisDISPA on `crates/ruff_python_formatter/src/statement/stmt_raise.rs`:24 on 2023-07-07 22:03_

I was using `Parenthesize::IfBreaks` at first but then I noticed that with black:

```python
# input

raise (aaaaa.aaa(a).a) from (aksjdhflsakhdflkjsadlfajkslhf)
raise bbbbb.bbb(bb) from c

# black output

raise (aaaaa.aaa(a).a) from (aksjdhflsakhdflkjsadlfajkslhf)
raise bbbbb.bbb(bb) from c

# ruff (IfBreak) ouput

raise aaaaa.aaa(a).a from aksjdhflsakhdflkjsadlfajkslhf
raise bbbbb.bbb(bb) from c
```

And I found that `Parenthesize::Optional` should work if I understand the doc correctly.  And I got:

```python
# ruff (Optional) output

raise (aaaaa.aaa(a).a) from (aksjdhflsakhdflkjsadlfajkslhf)
raise bbbbb.bbb(bb) from c
```

---

_Review comment by @LouisDISPA on `crates/ruff_python_formatter/src/statement/stmt_raise.rs`:24 on 2023-07-07 22:12_

But with your exemple I found another issue:
Black does not break if there is no parentheses but ruff does.

```python
# input

raise aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbb + cccccccccccccccccccccc + ddddddddddddddddddddddddd
raise (aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbb + cccccccccccccccccccccc + ddddddddddddddddddddddddd)

# black output

raise aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbb + cccccccccccccccccccccc + ddddddddddddddddddddddddd
raise (
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
    + bbbbbbbbbbbbbbbbbbbbbbbbbb
    + cccccccccccccccccccccc
    + ddddddddddddddddddddddddd
)

# ruff (Optional) ouput

raise (
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
    + bbbbbbbbbbbbbbbbbbbbbbbbbb
    + cccccccccccccccccccccc
    + ddddddddddddddddddddddddd
)
raise (
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
    + bbbbbbbbbbbbbbbbbbbbbbbbbb
    + cccccccccccccccccccccc
    + ddddddddddddddddddddddddd
)
```

And `Parenthesize::Preserve` does not work.
Is there something like: if in parenthesis then format else don't.

---

_@LouisDISPA reviewed on 2023-07-07 22:12_

---

_Review comment by @zanieb on `crates/ruff_python_formatter/src/statement/stmt_raise.rs`:24 on 2023-07-07 22:45_

I find it weird that black does not format these in the way that ruff does in the examples you shared 🤔 

---

_@zanieb reviewed on 2023-07-07 22:45_

---

_@LouisDISPA reviewed on 2023-07-08 09:56_

---

_Review comment by @LouisDISPA on `crates/ruff_python_formatter/src/statement/stmt_raise.rs`:24 on 2023-07-08 09:56_

I found the same behaviour with the lambda body:
```python
# input

lambda a: aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbb + cccccccccccccccccccccc + ddddddddddddddddddddddddd
lambda a: aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbb + (cccccccccccccccccccccc + ddddddddddddddddddddddddd)
lambda a: (aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbb + cccccccccccccccccccccc + ddddddddddddddddddddddddd)

# black output

lambda a: aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbb + cccccccccccccccccccccc + ddddddddddddddddddddddddd
lambda a: aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbb + (
    cccccccccccccccccccccc + ddddddddddddddddddddddddd
)
lambda a: (
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
    + bbbbbbbbbbbbbbbbbbbbbbbbbb
    + cccccccccccccccccccccc
    + ddddddddddddddddddddddddd
)

```


---

_Review comment by @LouisDISPA on `crates/ruff_python_formatter/src/statement/stmt_raise.rs`:24 on 2023-07-08 10:00_

and the yield value:
```python
# input

yield aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbb + cccccccccccccccccccccc + ddddddddddddddddddddddddd
yield aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbb + (cccccccccccccccccccccc + ddddddddddddddddddddddddd)
yield (aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbb + cccccccccccccccccccccc + ddddddddddddddddddddddddd)

# black output

yield aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbb + cccccccccccccccccccccc + ddddddddddddddddddddddddd
yield aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbb + (
    cccccccccccccccccccccc + ddddddddddddddddddddddddd
)
yield (
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
    + bbbbbbbbbbbbbbbbbbbbbbbbbb
    + cccccccccccccccccccccc
    + ddddddddddddddddddddddddd
)

```

---

_@LouisDISPA reviewed on 2023-07-08 10:00_

---

_Comment by @LouisDISPA on 2023-07-08 16:16_

I was looking at the same formatting of Black in other statements, I found `lambda a: [expr]`, `yield [expr]` and `with [expr]:` have the same behaviour as discussed [here](https://github.com/astral-sh/ruff/pull/5595#discussion_r1256547447).

The `with` statement is implemented but has a diff with black ([discussed here](https://github.com/astral-sh/ruff/issues/5368#issuecomment-1607688841)) and the solution seems to be quite complicated. I don't think I can resolve it, but if a fix is found I could maybe transfer it. I suppose there should be an option for the `Expr::format` to say "stay inline until there is a parenthesis".

Feel free to close the issue if the diff with black is not acceptable. I will continue to read the code but don't except me to find a solution. Love what you are doing, such an amazing work! Sorry for the inconvenience.

---

_Converted to draft by @LouisDISPA on 2023-07-10 07:22_

---

_Comment by @LouisDISPA on 2023-07-10 07:27_

I found some issues with stability, and I don't know how to solve it.

Input:
```python
raise ( # some comment
    # another one
)
```

First format:
```python
raise ( 
      # some comment
    # another one
)
```

Second format:
```python
raise ( 
    # some comment
    # another one
)
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_raise.rs`:24 on 2023-07-10 07:30_

Sorry, I confused `Parentheses::Optional` and `Parentheses::Preserve`. Optional should be fine here. I find black's behavior surprising. Either way, this is something you can ignore for your PR, because it affects all expression formatting.

---

_@MichaReiser reviewed on 2023-07-10 07:30_

---

_Comment by @MichaReiser on 2023-07-10 07:35_

> ```python
> raise ( # some comment
>     # another one
> )
> ```

@konstin what do you think. Should we make the trailing comment a leading comment of the `value`? Or should it be a dangling comment and we format it manually inside of the `raise` stmt? 

---

_Comment by @konstin on 2023-07-10 09:30_

i don't have a good intuition, i'm fine with merging whatever is stable. Function call e.g. currently format

```python
a = x( # b
    # c
)
```
to
```python
a = x()  # b
# c
```

I think we should add an empty parentheses comment util, unless someone else has specific ideas, i'll prototype something today or tomorrow that keeps both kinds of comments where they are for general empty parentheses

---

_Comment by @LouisDISPA on 2023-07-10 12:52_

I found the stability issue, it was in the tuple formatting implementation. I pushed a simple fix but if you want I can open a separate PR for the fix. 

I copied the `FormatExprList` implementation to fix it: [here](https://github.com/astral-sh/ruff/blob/bd8f65814c10b9592fabad4a4f7ba5bd2613c3a6/crates/ruff_python_formatter/src/expression/expr_list.rs#L25C9-L49C10)

---

_Comment by @konstin on 2023-07-10 16:06_

> I found the stability issue, it was in the tuple formatting implementation. I pushed a simple fix but if you want I can open a separate PR for the fix.

thank you! i'm fine with merging it with this PR with this fix

In general, is this PR ready to be merged?

---

_Comment by @LouisDISPA on 2023-07-10 17:05_

> > I found the stability issue, it was in the tuple formatting implementation. I pushed a simple fix but if you want I can open a separate PR for the fix.
> 
> thank you! i'm fine with merging it with this PR-
> 
> In general, is this PR ready to be merged?

Yes I'm putting this PR in ready status.

I also found the same issue with `FormatExprDict` formatting but I will open a different PR, I'm trying to find a cleaner way for handling dangling comments and share the implementation.

---

_Marked ready for review by @LouisDISPA on 2023-07-10 17:05_

---

_@konstin approved on 2023-07-10 19:23_

---

_Comment by @konstin on 2023-07-10 19:23_

> I also found the same issue with FormatExprDict formatting but I will open a different PR, I'm trying to find a cleaner way for handling dangling comments and share the implementation.

thanks, that would be great!

---

_Merged by @konstin on 2023-07-10 19:23_

---

_Closed by @konstin on 2023-07-10 19:23_

---
