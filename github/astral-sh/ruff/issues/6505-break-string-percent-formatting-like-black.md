---
number: 6505
title: Break string percent formatting like black
type: issue
state: closed
author: konstin
labels:
  - formatter
assignees: []
created_at: 2023-08-11T15:27:43Z
updated_at: 2023-08-24T07:29:07Z
url: https://github.com/astral-sh/ruff/issues/6505
synced_at: 2026-01-07T13:12:15-06:00
---

# Break string percent formatting like black

---

_Issue opened by @konstin on 2023-08-11 15:27_

black:
```python
template_params["lhs"] = "%s, %s" % (
    template_params,
    lookup_band_lhs,
)

template_params__lhs__ = "%s, %s" % (
    template_params,
    lookup_band_lhs,
)
```
ours:
```python
template_params["lhs"] = (
    "%s, %s"
    % (
        template_params,
        lookup_band_lhs,
    )
)

template_params__lhs__ = "%s, %s" % (
    template_params,
    lookup_band_lhs,
)
```

---

_Label `formatter` added by @konstin on 2023-08-11 15:27_

---

_Referenced in [astral-sh/ruff#6069](../../astral-sh/ruff/issues/6069.md) on 2023-08-11 15:28_

---

_Comment by @evanrittenhouse on 2023-08-12 03:12_

@konstin Feel free to assign this to me. May take a bit as I have to get up to speed with the formatter, though.

---

_Comment by @MichaReiser on 2023-08-12 04:45_

I'm not sure if this is a good first formatter issue. Feel free to have a look but I expect that this fix requires good knowledge of the IR

You could coordinate with @dhruvmanila and implement a match case formatting. 

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-16 07:14_

---

_Assigned to @konstin by @konstin on 2023-08-23 12:13_

---

_Comment by @konstin on 2023-08-24 07:22_

Fixed by #6815

---

_Closed by @konstin on 2023-08-24 07:22_

---

_Comment by @MichaReiser on 2023-08-24 07:29_

> Fixed by #6815

Uh interesting, that's not what I expected would fix this

---
