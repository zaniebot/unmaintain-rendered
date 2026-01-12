```yaml
number: 15130
title: Inconsistent formatting around power and brackets
type: issue
state: open
author: tobiasdiez
labels:
  - formatter
  - style
assignees: []
created_at: 2024-12-24T10:58:42Z
updated_at: 2024-12-24T13:55:25Z
url: https://github.com/astral-sh/ruff/issues/15130
synced_at: 2026-01-12T15:54:54Z
```

# Inconsistent formatting around power and brackets

---

_@tobiasdiez_

The expression 
```python
rho = sqrt(r**2 + a**2 * cos(th)**2)
```
is reformatted (`ruff format`) as
```python
rho = sqrt(r**2 + a**2 * cos(th) ** 2)
```

Note the additional space around `**` after the cosine.

---

_Label `formatter` added by @AlexWaygood on 2024-12-24 12:26_

---

_Comment by @AlexWaygood on 2024-12-24 13:26_

Thanks for the report! In general, Ruff's formatter tries to apply the same style to Python code as Black. I checked and Black [applies the same style here that we do](https://black.vercel.app/?version=stable&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4ACkAGhdAD2IimZxl1N_WmXYl8r1kM704aQABuAUFiA8L-5XUGJot2ttnwW-VBT_bJbFg6wHTF1oxNX2-KOKqLy58kABrAUxeXa3Oy41I9mMrVPbsQHP87qK_6mQQ2SDpmw47fjuyhHnQckPzCgAABlmBMa0z9C_AAGEAaUBAACjKGMAscRn-wIAAAAABFla). The motivation for this formatting is described in Black's documentation here: https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html#line-breaks-binary-operators. We've previously made chnages in the opposite direction to what you're asking for here, e.g. https://github.com/astral-sh/ruff/issues/7318, in order to ensure a compatible coding style with Black.

This doesn't mean that we _definitely won't_ change the style of how we format power expressions -- we do deviate from Black in _some_ areas. But deviations generally require very strong motivations and high levels of user demand, as we weigh compatibility with Black quite highly.

---

_Label `style` added by @MichaReiser on 2024-12-24 13:55_

---
