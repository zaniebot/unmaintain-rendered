```yaml
number: 13787
title: "[red-knot] Enhancing Diagnostics for Compare Expression Inference"
type: issue
state: closed
author: cake-monotone
labels:
  - ty
  - diagnostics
assignees: []
created_at: 2024-10-17T07:33:42Z
updated_at: 2024-11-07T15:11:11Z
url: https://github.com/astral-sh/ruff/issues/13787
synced_at: 2026-01-10T11:09:55Z
```

# [red-knot] Enhancing Diagnostics for Compare Expression Inference

---

_Issue opened by @cake-monotone on 2024-10-17 07:33_

This issue was suggested by @carljm and is related to the following threads: https://github.com/astral-sh/ruff/pull/13712#discussion_r1797193581, https://github.com/astral-sh/ruff/pull/13781#discussion_r1803637742, https://github.com/astral-sh/ruff/pull/13712#discussion_r1801739049

## Current Problems

Currently, the `infer_binary_type_comparison`, `infer_lexicographic_type_comparison`, and `perform_rich_comparison functions` in infer.rs return an `Option<Type>`, where `None` indicates that the comparison is not defined.

However, with the addition of comparison logic for Tuple and Union types, we’ve found that using `Option` may not provide sufficient diagnostic information.

For example, consider the following code:
```py
(1, 2) < (1, "hello")
```
The Python runtime provides an appropriate diagnostic message:

```
Traceback (most recent call last):
  File "/home/cake/works/playground/test.py", line 9, in <module>
    (1, 2) < (1, "hello")
TypeError: '<' not supported between instances of 'int' and 'str'
```

In contrast, our current `Option` implementation would result in:

```
Operator `<` is not supported for types `tuple[Literal[1], Literal[2]]` and `tuple[Literal[1], Literal["hello"]]`
```
(This is just my expected result; comparison between int and str isn’t implemented yet.)

## Implementation

I’m considering a straightforward change to the following structure. Any suggestions are welcome!

```rs
struct OperatorUnsupportedError<'db> {
    op: ast::CmpOp,
    lhs: Type<'db>,
    rhs: Type<'db>,
}

// ...

fn infer_binary_type_comparison(
    &mut self,
    left: Type<'db>,
    op: ast::CmpOp,
    right: Type<'db>,
) -> Result<Type<'db>, OperatorUnsupportedError<'db>>
```

---

_Label `red-knot` added by @MichaReiser on 2024-10-17 08:04_

---

_Comment by @carljm on 2024-10-17 22:12_

Yes, looks great, thank you for writing this up!

---

_Comment by @carljm on 2024-10-17 22:16_

One thing to consider is whether the diagnostic we want for your example is simply:

```
Operator `<` is not supported for types `Literal[2]` and `Literal["hello"]`
```

Or a more verbose variant where we give both the inner erroring types and the full types. Pyright gives both. I think we probably should as well, because the full outer type may also be inferred and non-obvious. So maybe something like:

```
Operator `<` is not supported for types `Literal[2]` and `Literal["hello"]`, in comparing `tuple[Literal[1], Literal[2]]` with `tuple[Literal[1], Literal["hello"]]`
```

---

_Comment by @cake-monotone on 2024-10-18 06:29_

I think there’s a subtle difference in the final diagnostic I expect.

```
Operator `<` is not supported for types `int` and `str`
```

As you know, in the current implementation, literals of different types are not compared directly. First, they are converted to `Type::Instance`, and then an Err will be returned and propagated.

So, the result would be:

```
Operator `<` is not supported for types `int` and `str`, in comparing `tuple[Literal[1], Literal[2]]` with `tuple[Literal[1], Literal["hello"]]`
```

It may not be intuitive because the user doesn’t know where the `int` and `str` are coming from. However, I still agree that providing both the inner and outer types is the better approach. I’ll start working on it as you suggested soon!

---

_Label `diagnostics` added by @MichaReiser on 2024-10-18 06:39_

---

_Comment by @MichaReiser on 2024-10-18 06:40_

Messages from other tools: 

TypeScript
`Operator '<' cannot be applied to types '[string, number]' and '[string, string]'.(2365)`

**Flow** (Nice! We should consider something with our new diagnostics)
```
    2:   a < b
         ^ Cannot compare tuple type [1] to tuple type [2]. [invalid-compare]
        References:
        1: function test(a: [string, number], b: [string, string]) {
                            ^ [1]
        1: function test(a: [string, number], b: [string, string]) {
                                                 ^ [2]
```

Pyright 

`Operator "<" not supported for types "tuple[Literal[1], Literal[2]]" and "tuple[Literal[1], Literal['hello']]"  (reportOperatorIssue)`

---

_Comment by @cake-monotone on 2024-10-18 08:42_

Thanks for reasearching this!!!!! Would it be possible to share an example of how Flow’s diagnostics could be applied to this case? I’m a little unsure about which parts of Flow’s diagnostics we should apply to.

By the way, pyright doesn't show inner-type either... :cry:

---

_Comment by @MichaReiser on 2024-10-18 08:46_

Here's a link to [flow's playground](https://flow.org/try/#1N4Igxg9gdgZglgcxALlAIwIZoKYBsD6uEEAztvhgE6UYCe+JADpdhgCYowa5kA0I2KAFcAtiRQAXSkOz9sADwxgJ+NPTbYuQ3BMnTZA+Y2yU4IwRO4A6SFBIrGVDGM7c+h46fNRLuKxJIGWh8MeT0ZfhYlCStpHzNsFBAMIQkIEQwJODAQfiEyfBE4eWw2fDgofDBMsAALfAA3KjgsXGxxZC4eAw0G-GhcWn9aY3wWZldu-g1mbGqJUoBaCRHEzrcDEgBrbAk62kXhXFxJ923d-cPRHEpTgyEoMDaqZdW7vKgoOfaSKgOKpqmDA+d4gB5fMA-P6LCCMLLQbiLOoYCqgh6-GDYRYIXYLSgkRZkCR4jpddwPfJLZjpOBkO4AX34kA0SRgD2UcGgAAIFvYABQYZBcgDawhEN14XPspigCAAupK0ELhdKKghJarZXKAJRCtDEZ5QLnAAA6Rq5FpYEiElCNGC5AB4uWgzfTciAGiYSJyoEkGgAGKwAJgALABOKz+kD0oA)

What I like about flow is that it renders the longer types, including those declared outside of the message header. I would have to install their CLI to see how that looks. But I do like that they don't try to squeeze everything into the message.

---

_Comment by @carljm on 2024-10-18 18:22_

I think something like Flow's diagnostic would be great, once we have a more complete diagnostics system in place that allows formatting code frames like that; I definitely wouldn't try to implement that now.

I guess the one thing it suggests is that maybe the error return value should include both the erroring types and their nodes (or at least node locations). But I also think probably we don't need to worry about that yet, either. The main thing is to change the return value to `Result` and propagate it correctly; if we do that, it's not hard to add more information to the Err variant in future once we have a new diagnostics system in place. 

---

_Comment by @carljm on 2024-10-18 18:23_

> By the way, pyright doesn't show inner-type either

Weird, I definitely thought I recently saw a pyright diagnostic for a case like this, where it stacked multiple errors for the innermost vs outer types. But now I can't figure out what code example I was looking at that produced that.

---

_Closed by @carljm on 2024-11-07 15:11_

---
