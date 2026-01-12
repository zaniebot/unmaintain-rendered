```yaml
number: 8546
title: "Formatter: long walrus operator expression introduces parentheses "
type: issue
state: open
author: tjkuson
labels:
  - formatter
  - style
assignees: []
created_at: 2023-11-07T18:37:48Z
updated_at: 2024-02-15T09:47:08Z
url: https://github.com/astral-sh/ruff/issues/8546
synced_at: 2026-01-12T15:54:48Z
```

# Formatter: long walrus operator expression introduces parentheses 

---

_@tjkuson_

This seems similar to #7246 which was closed as completed, but reproducible in the latest `0.1.4` release.

Input:

```python
if True:
    if True:
        if True:
            if True:
                if foo := some.very.very.very.very.very.very.very.very.very.long.function(
                    search.study_id
                ):
                    pass
```

Ruff formatter output:

```python
if True:
    if True:
        if True:
            if True:
                if (
                    foo
                    := some.very.very.very.very.very.very.very.very.very.long.function(
                        search.study_id
                    )
                ):
                    pass
```

Black doesn't make any changes to the input file.

[Ruff playground](https://play.ruff.rs/c29224dd-283a-4c24-b96a-68f1da225743)


---

_Comment by @tjkuson on 2023-11-07 18:43_

Behaviour is the same for long method chains.

Input:

```python
if True:
    if True:
        if True:
            if True:
                if foo == some.very.very.very.very.very.very.very.very.long_function_name(
                    bar
                ):
                    pass
```

Output:

```python
if True:
    if True:
        if True:
            if True:
                if (
                    foo
                    == some.very.very.very.very.very.very.very.very.long_function_name(
                        bar
                    )
                ):
                    pass
```

EDIT: I marked this as outdated as the revised reproduction in the original post now details this behaviour.

---

_Label `formatter` added by @charliermarsh on 2023-11-07 22:38_

---

_Comment by @charliermarsh on 2023-11-07 22:39_

I see the same behavior in Black, or am I misunderstanding?

https://black.vercel.app/?version=main&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4AFGAIRdAD2IimZxl1N_Wlws4TBexXdmg613D2cRmmkK6QFkhhlNwTAUx4cz8BUeWx-cuwgYYrUzOEI2SVBXlwXRQgtbOqCvafh3iEcmeVpQMwvjow_MFJgqFPO0sgLW_4N7UAXeDoEHF1scyvxK89Ro-KbTojpHo8ZMycPjVc16iGNlSFE4PBe88ADJBimajoVexAABoAHHAgAAUu43D7HEZ_sCAAAAAARZWg==

---

_Comment by @tjkuson on 2023-11-07 22:54_

Sorry, I made an error in the reproduction!

Input:

```python
if True:
    if True:
        if True:
            if True:
                if foo := some.very.very.very.very.very.very.very.very.very.long.function(
                    search.study_id
                ):
                    pass
```

Ruff

```python
if True:
    if True:
        if True:
            if True:
                if (
                    foo
                    := some.very.very.very.very.very.very.very.very.very.long.function(
                        search.study_id
                    )
                ):
                    pass
```

Black

```python
if True:
    if True:
        if True:
            if True:
                if foo := some.very.very.very.very.very.very.very.very.very.long.function(
                    search.study_id
                ):
                    pass
```

---

_Comment by @tjkuson on 2023-11-07 22:56_

(I've updated the original post to include the proper reproduction.)

---

_Renamed from "Formatter: long comparison introduces parentheses " to "Formatter: long assignment expression introduces parentheses " by @tjkuson on 2023-11-07 22:58_

---

_Label `needs-decision` added by @charliermarsh on 2023-11-08 00:51_

---

_Renamed from "Formatter: long assignment expression introduces parentheses " to "Formatter: long walrus operator expression introduces parentheses " by @MichaReiser on 2023-11-27 11:34_

---

_Comment by @MichaReiser on 2023-11-27 11:35_

It seems that black formats the walrus operator more closely to assignments where it only breaks the right but never the left (or before/after the operator), even if doing so would help fitting the content on the line. 

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-11-27 11:35_

---

_Label `style` added by @MichaReiser on 2023-11-28 06:30_

---

_Removed from milestone `Formatter: Stable` by @MichaReiser on 2024-02-13 11:39_

---

_Label `needs-decision` removed by @MichaReiser on 2024-02-15 09:47_

---
