```yaml
number: 7463
title: "Formatter instability: Long string"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-09-17T15:07:42Z
updated_at: 2023-09-19T06:29:08Z
url: https://github.com/astral-sh/ruff/issues/7463
synced_at: 2026-01-12T15:54:47Z
```

# Formatter instability: Long string

---

_@MichaReiser_

## Input

```python
mp3fmt="<span style=\"color: grey\"><a href=\"{}\" id=\"audiolink\">listen</a></span></br>\n"
```

## Format 1

```python
mp3fmt = '<span style="color: grey"><a href="{}" id="audiolink">listen</a></span></br>\n'
```

## Format 2

```python
mp3fmt = (
    '<span style="color: grey"><a href="{}" id="audiolink">listen</a></span></br>\n'
)
```

This could be due to the optimization where strings don't use the BestFit layout when they exceed a certain length (Format 1, but Format 2 uses best fit)


Sourced by #7445

---

_Label `bug` added by @MichaReiser on 2023-09-17 15:07_

---

_Label `formatter` added by @MichaReiser on 2023-09-17 15:07_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-17 15:07_

---

_Assigned to @MichaReiser by @MichaReiser on 2023-09-18 13:32_

---

_Closed by @MichaReiser on 2023-09-19 06:29_

---
