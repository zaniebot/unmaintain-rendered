```yaml
number: 7422
title: "Formatter incompatibility: parenthesized subscript access"
type: issue
state: closed
author: charliermarsh
labels:
  - formatter
  - accepted
assignees: []
created_at: 2023-09-15T21:11:01Z
updated_at: 2023-09-16T14:21:47Z
url: https://github.com/astral-sh/ruff/issues/7422
synced_at: 2026-01-12T15:54:47Z
```

# Formatter incompatibility: parenthesized subscript access

---

_@charliermarsh_

Given:

```python
if True:
    if True:
        if True:
            if True:
                if True:
                    self.intermediator_address = (
                        invoice_recipient_intermediator_address[0].text
                    )
```

Ruff formats as:

```python
if True:
    if True:
        if True:
            if True:
                if True:
                    self.intermediator_address = invoice_recipient_intermediator_address[
                        0
                    ].text
```

Whereas Black formats as:

```python
if True:
    if True:
        if True:
            if True:
                if True:
                    self.intermediator_address = (
                        invoice_recipient_intermediator_address[0].text
                    )
```

(Sorry for all the nesting, easiest way to strip context while preserving the line lengths.)

Sourced from https://github.com/astral-sh/ruff/issues/7394.


---

_Renamed from "Formatter incompatibility:" to "Formatter incompatibility: parenthesized subscript access" by @charliermarsh on 2023-09-15 21:11_

---

_Label `formatter` added by @charliermarsh on 2023-09-15 21:11_

---

_Added to milestone `Formatter: Beta` by @charliermarsh on 2023-09-15 21:11_

---

_Label `needs-decision` added by @charliermarsh on 2023-09-15 21:11_

---

_Comment by @zanieb on 2023-09-15 21:12_

I prefer the Black formatting here.

---

_Comment by @MichaReiser on 2023-09-15 21:15_

Could be fixed by https://github.com/astral-sh/ruff/pull/7409

---

_Comment by @charliermarsh on 2023-09-15 21:16_

I'll check.

---

_Comment by @charliermarsh on 2023-09-15 21:17_

Confirmed.

---

_Label `needs-decision` removed by @charliermarsh on 2023-09-15 21:17_

---

_Label `accepted` added by @charliermarsh on 2023-09-15 21:17_

---

_Assigned to @MichaReiser by @charliermarsh on 2023-09-15 21:17_

---

_Closed by @MichaReiser on 2023-09-16 14:21_

---
