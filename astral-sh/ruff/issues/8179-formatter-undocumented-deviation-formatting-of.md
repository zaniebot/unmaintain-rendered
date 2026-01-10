```yaml
number: 8179
title: "Formatter undocumented deviation: Formatting of long lambda as keyword argument "
type: issue
state: closed
author: henribru
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-10-24T19:42:12Z
updated_at: 2025-12-12T17:03:39Z
url: https://github.com/astral-sh/ruff/issues/8179
synced_at: 2026-01-10T11:09:50Z
```

# Formatter undocumented deviation: Formatting of long lambda as keyword argument 

---

_Issue opened by @henribru on 2023-10-24 19:42_

[Black formatting](https://black.vercel.app/?version=stable&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4AEtAIddAD2IimZxl1N_WlbvK5V8G17R4xukEnqOHRjoYorGA3_x5d79TklUYR_XtF6MM4d5srbPV3-N4pXQ1SwF_YZZxkksYNb5k5P7chlg8SSpdJuccpW53I3EE2uXGr5zBl09V3WMIRMwonXPxdgmzF3oapEcKjKrbacGLJ8ijg5fK84E8SAp_bjGAAAA8DH6Gx1SCp8AAaMBrgIAAHUkjc-xxGf7AgAAAAAEWVo=):
```
def a():
    return b(
        c,
        d,
        e,
        f=lambda self, *args, **kwargs: aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa(
            *args, **kwargs
        ),
    )
```

[Ruff formatting](https://play.ruff.rs/7f9b066a-9a46-4976-86d3-c872850310d3?secondary=Format):
```
def a():
    return b(
        c,
        d,
        e,
        f=lambda self,
        *args,
        **kwargs: aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa(*args, **kwargs),
    )
```

In addition to being incompatible, this formatting seems very strange and not very readable, as the `*args` and `**kwargs` suddenly look like they are arguments to `b`.

---

_Renamed from "Formatter undocumented deviation: " to "Formatter undocumented deviation: Formatting of long lambda as keyword argument " by @henribru on 2023-10-24 19:42_

---

_Comment by @charliermarsh on 2023-10-24 19:59_

Thanks!

---

_Label `formatter` added by @charliermarsh on 2023-10-24 19:59_

---

_Added to milestone `Formatter: Stable` by @charliermarsh on 2023-10-24 19:59_

---

_Label `bug` added by @charliermarsh on 2023-10-24 19:59_

---

_Comment by @henribru on 2023-10-24 20:51_

This seems possibly related

https://black.vercel.app/?version=stable&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4ADqAHRdAD2IimZxl1N_WlXnON2nzNX9HFvHx6cBUQ15-GcjjCXTWY4lAb3LPNtN0kban15sEExCiEMHC1fqLMN_I2mNYrKs6DX7rL7K_IqhZ4f1qfF_pwoKJfLbTWpcKTtsPNouVDJpGY10D5nlGuf_NfXkJrKjOtjHACUnbTAaUKR-AAGQAesBAAAgR-_jscRn-wIAAAAABFla

https://play.ruff.rs/c9b4e3fa-4401-4248-a98d-d9ae6b984697?secondary=Format

---

_Comment by @charliermarsh on 2023-10-24 20:58_

Yeah, looks like we have some refinements to make in the lambda formatting. On it! (Thanks for sharing playground links, it helps a lot.)

---

_Comment by @DarthMuzammil on 2023-10-24 21:03_

@charliermarsh Can i take this issue up?

---

_Comment by @MichaReiser on 2023-10-24 23:41_

@DarthMuzammil sure. Let me know if you need any help

---

_Assigned to @DarthMuzammil by @MichaReiser on 2023-10-24 23:41_

---

_Comment by @charliermarsh on 2023-10-29 16:24_

@DarthMuzammil -- have you had a chance to look into this? If not, do you still plan to? (No pressure, I just want to make sure we find an owner soon if it's not being addressed.)

---

_Comment by @MichaReiser on 2023-11-02 07:14_

It seems black never inserts soft line breaks between parameters. This would be an easy fix... except for comments

Although Black's formatting also has its shortcomings when you have default values:

```python
def a():
    return b(
        c,
        d,
        e,
        f=lambda self, araa, kkkwargs=aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa(
            *args, **kwargs
        ), e=1, f=2, g=2: d,
    )

```

It could be nice to simply parenthesize the lambda expression and indent it by 1 level (and otherwise keep our formatting). However, that would also mean that the formatter must remove the parentheses again when the lambda fits on a single line—something we haven't really done before in sub-expression positions. And there's also no real good place to add it, because this should be done when formatting lambas in any position. 

---

_Comment by @DarthMuzammil on 2023-11-02 13:07_

@charliermarsh  Hey, sorry for the delay i just saw this message, yes i will start working on this today

---

_Comment by @DarthMuzammil on 2023-11-02 18:50_

> It seems black never inserts soft line breaks between parameters. This would be an easy fix... except for comments
> 
> Although Black's formatting also has its shortcomings when you have default values:
> 
> ```python
> def a():
>     return b(
>         c,
>         d,
>         e,
>         f=lambda self, araa, kkkwargs=aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa(
>             *args, **kwargs
>         ), e=1, f=2, g=2: d,
>     )
> ```
> 
> It could be nice to simply parenthesize the lambda expression and indent it by 1 level (and otherwise keep our formatting). However, that would also mean that the formatter must remove the parentheses again when the lambda fits on a single line—something we haven't really done before in sub-expression positions. And there's also no real good place to add it, because this should be done when formatting lambas in any position.

@MichaReiser How should i go about solving this issue?

---

_Comment by @MichaReiser on 2023-11-03 02:16_

@DarthMuzammil not sure. I think coming up with a style that works with the presence of comments (which black does not always) is the main work of this issue. My preferred solution would be to just parenthesize the lambda parameters if it breaks, but Python3 removed support for parenthesized lambda parameters for some reason :(

```python
# Using parentheses is a syntax error
def a():
    return b(
        c,
        d,
        e,
        f=lambda (
            self, 
            araa, 
            kkkwargs=aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa(
                *args, **kwargs
            ), 
            e=1, 
            f=2, 
            g=2
        ): d,
    )
```

Related black issue: https://github.com/psf/black/issues/809

---

_Unassigned @DarthMuzammil by @MichaReiser on 2023-11-28 06:33_

---

_Removed from milestone `Formatter: Stable` by @MichaReiser on 2024-02-13 11:38_

---

_Closed by @ntBre on 2025-12-12 17:02_

---

_Comment by @MichaReiser on 2025-12-12 17:03_

@ntBre just realized. We shouldl update our black deviation documentation and explain our new lambda formatting (or update the existing section). 

---
