```yaml
number: 8748
title: "Formatter undocumented deviation: Trailing comma added after kwargs"
type: issue
state: closed
author: henribru
labels:
  - documentation
  - formatter
assignees: []
created_at: 2023-11-17T22:25:04Z
updated_at: 2023-11-18T21:10:23Z
url: https://github.com/astral-sh/ruff/issues/8748
synced_at: 2026-01-10T11:09:51Z
```

# Formatter undocumented deviation: Trailing comma added after kwargs

---

_Issue opened by @henribru on 2023-11-17 22:25_

[Black:](https://black.vercel.app/?version=stable&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4ADhAGNdAD2IimZxl1N_WlbvK5V9KEd00teEET9osIZigniJF5fr_T7GfCbcGUBQhUU3aA1epEd495EVTT5WLbOM4upLfhfjwWg9Cp0Mh8h9sbfEEDGcq8Z-myt2zvNgeXO5vo1Vmkx7AAAAoJ0uR6yEDA8AAX_iAQAAABOYidOxxGf7AgAAAAAEWVo=)
```
def foo(
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa,
    b,
    **kwargs
):
    ...
```

[Ruff:](https://play.ruff.rs/1e68e379-2427-44ae-ace8-53bd5a937b45?secondary=Format)
```
def foo(
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa,
    b,
    **kwargs,
):
    ...
```

Not sure if this is somehow related to https://github.com/astral-sh/ruff/blob/main/docs/formatter/black.md#trailing-commas-are-inserted-when-expanding-a-function-definition-with-a-single-argument, but there's more than one argument here at least.

---

_Comment by @charliermarsh on 2023-11-17 22:27_

I believe this is intentional as we view Black's behavior here as inconsistent, but it should be documented.

---

_Label `documentation` added by @charliermarsh on 2023-11-17 22:27_

---

_Label `formatter` added by @charliermarsh on 2023-11-17 22:27_

---

_Comment by @charliermarsh on 2023-11-18 01:19_

I'm actually unable to reproduce this with the Black executable, only in the playground under certain circumstances (e.g., see [here](https://black.vercel.app/?version=main&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4AFfAG9dAD2IimZxl1N_WlbvK5V9KEd00teEET9osIZigniJF5fr_T7GfCbcGUBQhUU3aA1epE1XHBGrTi7yEbaaNnAWgzoeI31q1coFWupDVR4mT6KoJs0brDOpVkjAfO4WtqJ8JpfwvuPoOVZqyN70iB9QAAAAYOgxoiNok58AAYsB4AIAACCMuV-xxGf7AgAAAAAEWVo=)). Is this something you experienced in one of your projects?

---

_Comment by @henribru on 2023-11-18 07:53_

> I'm actually unable to reproduce this with the Black executable, only in the playground under certain circumstances (e.g., see [here](https://black.vercel.app/?version=main&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4AFfAG9dAD2IimZxl1N_WlbvK5V9KEd00teEET9osIZigniJF5fr_T7GfCbcGUBQhUU3aA1epE1XHBGrTi7yEbaaNnAWgzoeI31q1coFWupDVR4mT6KoJs0brDOpVkjAfO4WtqJ8JpfwvuPoOVZqyN70iB9QAAAAYOgxoiNok58AAYsB4AIAACCMuV-xxGf7AgAAAAAEWVo=)). Is this something you experienced in one of your projects?

I can reproduce my example with the executable. It also reproduces if I duplicate the function. But your example shows something curious. It seems like if none of the functions have trailing comma on kwargs it doesn't get added, but if one of them has it it gets added for all of them? 🤔 

---

_Comment by @henribru on 2023-11-18 08:09_

Ahh, I see what's going on. Trailing comma on kwargs is a syntax error before Python 3.6. If you have a trailing comma already, without a specified target version, Black is inferring the code must be >=3.6, so it adds it to the rest as well. Since Ruff doesn't infer the target version (and in any case has 3.7 as the minimum), this doesn't apply to Ruff. My bad!

---

_Closed by @henribru on 2023-11-18 08:09_

---

_Comment by @henribru on 2023-11-18 08:13_

Though maybe a quick note in the documentation about how if you're relying on Black inferring the target version you might see differences when migrating to Ruff could be useful

---

_Comment by @charliermarsh on 2023-11-18 21:10_

Ahh that explains a lot! Thank you for the follow-up.

---
