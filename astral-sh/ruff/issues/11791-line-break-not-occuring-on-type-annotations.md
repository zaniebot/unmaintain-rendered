```yaml
number: 11791
title: Line break not occuring on type annotations whereas black does
type: issue
state: open
author: jonathan-hill-visasq
labels:
  - breaking
  - formatter
  - accepted
assignees: []
created_at: 2024-06-07T04:00:54Z
updated_at: 2024-06-07T07:02:50Z
url: https://github.com/astral-sh/ruff/issues/11791
synced_at: 2026-01-10T11:09:53Z
```

# Line break not occuring on type annotations whereas black does

---

_Issue opened by @jonathan-hill-visasq on 2024-06-07 04:00_

## Issue Description
I've noticed a deviation from the black formatter, which I can't find accounted for in either the [known deviations](https://docs.astral.sh/ruff/formatter/black/) page, or the [issue tracker](https://github.com/astral-sh/ruff/issues?q=is%3Aopen+is%3Aissue+label%3Aformatter).

It occurs on assignments where
- there is a type annotation 
- the maximum line length is exceeded in the middle of the type annotation.

Black (24.4.2), will parenthesize the type annotation and break the line, ensuring no lines exceed the line length.
Ruff (0.4.7) however keeps the assignment all one line. Even if the file is formatted by black, if `ruff format` is run after, it removes the parenthesis and puts it all back on one (too long) line.

Black started parenthesizing and line breaking on type annotations as of version [24.1.0](https://black.readthedocs.io/en/stable/change_log.html#id18), specifically PR [3899](https://github.com/psf/black/pull/3899).

## Example
Example (default line-length of 88) - script.py(extract):


```
# input
a_member_with_long_long_name: TypeNameWhichIsLongEnoughToCauseLineLengthToBeOverLimit = 12345

# ruff-format output
a_member_with_long_long_name: TypeNameWhichIsLongEnoughToCauseLineLengthToBeOverLimit = 12345

# black output
a_member_with_long_long_name: (
    TypeNameWhichIsLongEnoughToCauseLineLengthToBeOverLimit
) = 12345

```
commands used:
- `black script.py` `ruff-format script.py`

## Related Links/Discussions
The original [issue](https://github.com/psf/black/issues/2316) that led to the black change (v24.1.0, PR#3899) , only seems to specify type compound type annotations- ie `int | str`, rather than `int` or `str`. When I try a compound type annotation with ruff, it does indeed parenthesize and line-break the same as black. 
So the deviation only occurs when the type annotation is comprised of a single type only. 

Is this deviation intentional? Given that the original black issue only seems to reference compound type annotations, I guess it could be argued that the ruff output is now "correct" - but personally I would strongly prefer having the annotation parenthesized and line-broken

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Label `formatter` added by @MichaReiser on 2024-06-07 05:52_

---

_Comment by @MichaReiser on 2024-06-07 06:13_

Hi, thanks for the very detailed write up and linking to all relevant issues! 

A few more observations to the deviation:

For the given example, the variable name and the type annotation exceed the line length limit. This is even before the value begins. 

Both Ruff and Black format the assignment the same if the variable name and type annotation fit into the line length

```python
a_member_with_long_long_name: TypeNameWhichIsLongEnoughToCauseLineLengthToBeOdddd = (
    12345
)
```

Both Ruff and Black collapse the value when parenthesizing it doesn't help fitting it in the configured line length

```python
a_member_with_long_long_name: TypeNameWhichIsLongEnoughToCauseLineLengthToBeOddd = 12345
```

Arguably Black's formatting is more consistent. Let's say we start with

```python
a_member_with_long_long_name: TypeNameWhichIsLongEnoughToCauseLineLengthToBeOddddddddddddddd 
```

Ruff and Black both parenthesize the type annotation because it exceeds the configured line length

```python
a_member_with_long_long_name: (
    TypeNameWhichIsLongEnoughToCauseLineLengthToBeOddddddddddddddd
)
```

Black keeps the parentheses when we add a value, but ruff removes them, leading to a larger diff than necessary. That's why I think should fix this, but I don't think we can before the next stable style. 

I'm not sure how easy this fix is, considering that the assignment expression is already fairly complicated and Black has a lot of special casing. For example, Black formats

```python
a_member_with_long_long_name: TypeNameWhichIsLongEnoughToCauseLineLengthToBeOddddddddddddddd # comment
```

to 

```python
a_member_with_long_long_name: (
    TypeNameWhichIsLongEnoughToCauseLineLengthToBeOddddddddddddddd  # comment
)
```

which Ruff supports too. But I guess this behavior is not relevant when there's a value?



https://github.com/astral-sh/ruff/blob/a6f32ddc5e9582112e76ad25039e9b080e281dce/crates/ruff_python_formatter/src/statement/stmt_ann_assign.rs#L47-L52

My simple fix of replacing these lines with

```rust
 else {
      optional_parentheses(&annotation.format().with_options(Parentheses::Never))
          .fmt(f)?;
  }
```

Doesn't work because the annotation now splits before the value. So I fear we would need to integrate this into `left_to_right` somehow.



---

_Label `breaking` added by @MichaReiser on 2024-06-07 06:16_

---

_Label `accepted` added by @MichaReiser on 2024-06-07 06:16_

---

_Comment by @jonathan-hill-visasq on 2024-06-07 06:59_

@MichaReiser Thanks for the quick response!
Unfortunately I'm not familiar with the ruff code base, or even rust myself, so trying to fix this would be too big of a commitment for me to take on now... but it's good to know it is a genuine deviation.

---

_Comment by @MichaReiser on 2024-06-07 07:02_

@jonathan-hill-visasq yeah sorry. It wasn't my intention to imply that you should be fixing it. I mainly added the notes to know the complexity and where to start if I get back to the issue (or someone else who's interested). But thanks for considering it.

---
