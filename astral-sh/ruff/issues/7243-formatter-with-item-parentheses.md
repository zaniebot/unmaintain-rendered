```yaml
number: 7243
title: "Formatter: With Item parentheses"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - formatter
  - accepted
assignees: []
created_at: 2023-09-08T10:53:24Z
updated_at: 2023-10-04T04:23:33Z
url: https://github.com/astral-sh/ruff/issues/7243
synced_at: 2026-01-12T15:54:46Z
```

# Formatter: With Item parentheses

---

_@MichaReiser_

## Input

```python
if True:
    with (
        anyio.CancelScope(shield=True)
        if get_running_loop()
        else contextlib.nullcontext()
    ):
        pass

if True:
    with (
        anyio.CancelScope(shield=True)
        and B and [aaaaaaaa, bbbbbbbbbbbbb, cccccccccc, dddddddddddd, eeeeeeeeeeeee]
    ):
        pass
```

## Black

```python
if True:
    with (
        anyio.CancelScope(shield=True)
        if get_running_loop()
        else contextlib.nullcontext()
    ):
        pass

if True:
    with (
        anyio.CancelScope(shield=True)
        and B
        and [aaaaaaaa, bbbbbbbbbbbbb, cccccccccc, dddddddddddd, eeeeeeeeeeeee]
    ):
        pass
```

## Ruff

```python
if True:
    with anyio.CancelScope(
        shield=True
    ) if get_running_loop() else contextlib.nullcontext():
        pass

if True:
    with anyio.CancelScope(shield=True) and B and [
        aaaaaaaa,
        bbbbbbbbbbbbb,
        cccccccccc,
        dddddddddddd,
        eeeeeeeeeeeee,
    ]:
        pass
```



## Expected

Ruff uses the `maybe_parenthesize` (can omit optional parentheses) layout where black seems not to. 

I'm not sure if I agree with this choice because Black/Ruff use the `maybe_parenthesize` layout consistently in all other clause header positions. 




---

_Label `formatter` added by @MichaReiser on 2023-09-08 10:53_

---

_Label `needs-decision` added by @MichaReiser on 2023-09-08 10:53_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-08 10:53_

---

_Comment by @charliermarsh on 2023-09-27 15:00_

This is a bug -- we should be preserving the parentheses. We do the right thing with `if` statements:

```python
if True:
    with (
        anyio.CancelScope(shield=True)
        if get_running_loop()
        else contextlib.nullcontext()
    ):
        pass

if True:
    if (
        anyio.CancelScope(shield=True)
        if get_running_loop()
        else contextlib.nullcontext()
    ):
        pass
```

---

_Label `needs-decision` removed by @charliermarsh on 2023-09-27 15:00_

---

_Label `bug` added by @charliermarsh on 2023-09-27 15:00_

---

_Removed from milestone `Formatter: Beta` by @MichaReiser on 2023-09-27 15:01_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-09-27 15:01_

---

_Comment by @charliermarsh on 2023-09-27 15:01_

Not a blocker for the Beta, but we may fix it anyway this month.

---

_Label `accepted` added by @charliermarsh on 2023-09-27 18:50_

---

_Comment by @charliermarsh on 2023-09-28 00:08_

Not totally trivial in part due to the `as`.

---

_Assigned to @MichaReiser by @MichaReiser on 2023-09-28 09:55_

---

_Comment by @charliermarsh on 2023-10-04 04:23_

I believe this was closed by https://github.com/astral-sh/ruff/pull/7694.

---

_Closed by @charliermarsh on 2023-10-04 04:23_

---
