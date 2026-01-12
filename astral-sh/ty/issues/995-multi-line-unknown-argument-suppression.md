```yaml
number: 995
title: multi-line unknown-argument suppression
type: issue
state: open
author: CharlesPerrotMinot
labels:
  - suppression
assignees: []
created_at: 2025-08-15T04:55:44Z
updated_at: 2025-08-18T17:13:49Z
url: https://github.com/astral-sh/ty/issues/995
synced_at: 2026-01-12T15:54:24Z
```

# multi-line unknown-argument suppression

---

_@CharlesPerrotMinot_

### Summary

Hi,

According to the docs, we should be able to do multiline suppressions:
https://docs.astral.sh/ty/suppression/#ty-suppression-comments

But this doesn't seem to work with unknown-argument.
This still errors out:
```
MyBuilder(
    name="123",
    translation="Some translation",
)  # ty: ignore[unknown-argument]
```

Same as:
```
MyBuilder(  # ty: ignore[unknown-argument]
    name="123",
    translation="Some translation",
)
```

Only this works:
```
MyBuilder(
    name="123",  # ty: ignore[unknown-argument]
    translation="Some translation",  # ty: ignore[unknown-argument]
)
```

For more context: https://github.com/astral-sh/ty/issues/988#issuecomment-3189107887

### Version

0.0.1-alpha.18

---

_Comment by @MichaReiser on 2025-08-15 06:21_

Here's a more complete example: https://play.ty.dev/071a7857-df0c-42a5-801c-d2a59e2440d4 and I'll use this to explain what's happening. 

The first diagnostic is:

```
Argument `name` does not match any known parameter of bound method `__init__` (unknown-argument) [Ln 7, Col 5]
```

and the documentation says:

> Rule violations spanning multiple lines can be suppressed by adding the comment at the end of the violation's first or last line:

The violation's first line is line number 7 as you can see in the diagnostic message. That means, you can place a suppression comment at the end of line 7. 

Now, it's a bit harder to tell where a diagnostic ends because that's not visible in its message but in this instance, it's also line number 7. That means, suppressions are only allowed at the end of line 7.


You can change your example to:

```py
class MyBuilder:

    def __init__(self):
        pass

MyBuilder(
    name=  # ty: ignore[unknown-argument]
        "123",
    translation="Some translation",
) 

```

In which case the suppression is both valid at the end of the `name=` line or after `"123",`

```py
class MyBuilder:

    def __init__(self):
        pass

MyBuilder(
    name=
        "123",   # ty: ignore[unknown-argument]
    translation="Some translation",
)  
```

Because the diagnostic starts at line 7, and ends at line 8, suppressions are valid at both the end of line 7 and 8

You can find more exmaples in https://github.com/astral-sh/ruff/blob/1167ed61cf3fa186e852df2d6ec9cb0136dffa80/crates/ty_python_semantic/resources/mdtest/suppressions/ty_ignore.md

Now, I can see how it might be desirable to suppress all unknown argument errors with a single suppression. And we had other instances where we intentionally kept the diagnostic range larger than it had to be so that suppressions worked more intuitively (@AlexWaygood do you remember some of those instances). What we discussed there is whether diagnostics can have a secondary range that isn't visible when rendering the diagnostic but adds an extra range where suppressions are allowed in addition to the start and end line of the diagnostic itself.

---

_Label `suppression` added by @MichaReiser on 2025-08-15 06:21_

---

_Comment by @AlexWaygood on 2025-08-18 17:13_

> we had other instances where we intentionally kept the diagnostic range larger than it had to be so that suppressions worked more intuitively ([@AlexWaygood](https://github.com/AlexWaygood) do you remember some of those instances)

Yeah, some of the other instances where we intentionally kept the range broader than what we'd ideally highlight in the diagnostic rendering (so that suppresion comments work intuitively) are:
- `assert_type`
- `assert_never`
- `cast`

For `reveal_type` we did not do this -- we changed the range of the diagnostic so that it was only the range of one of the arguments passed in. I think that was the correct decision, because users will never realistically want to suppress a `reveal-type` diagnostic. But the narrower diagnostic ranges still caused issues for _us_ when writing mdtests: https://github.com/astral-sh/ruff/pull/17980/files#r2081633694

---
