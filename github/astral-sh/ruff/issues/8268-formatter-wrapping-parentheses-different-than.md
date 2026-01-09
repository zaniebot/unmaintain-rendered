---
number: 8268
title: "Formatter: Wrapping Parentheses Different than Black"
type: issue
state: closed
author: juftin
labels:
  - formatter
assignees: []
created_at: 2023-10-27T02:20:29Z
updated_at: 2023-10-30T00:47:36Z
url: https://github.com/astral-sh/ruff/issues/8268
synced_at: 2026-01-07T13:12:15-06:00
---

# Formatter: Wrapping Parentheses Different than Black

---

_Issue opened by @juftin on 2023-10-27 02:20_

## Summary

Black and Ruff formatted this code differently - I don't think this is a Known Deviation and could't find an similar issue.

Both are readable but I actually prefer what `black` is doing here on `YELLOWSTONE_CAMPSITE_AVAILABILITY` and `YELLOWSTONE_PROPERTY_INFO`. I prefer the variable name and the type annotation on the same line as opposed to the variable name, annotation, and value on three separate lines. I also prefer the parentheses to be around the value instead of the annotation.

I came across the issue when moving from black to ruff on a large code base. Everything else looked perfect!

## Original Code

```python
class YellowstoneConfig:
    """
    Yellowstone Variable Storage Class
    """

    API_BASE_PATH: str = "v1/api"
    YELLOWSTONE_PARK_PATH: str = "yellowstonenationalparklodges"
    CAMPSITE_AVAILABILITY: str = f"{API_BASE_PATH}/availability/rooms"
    YELLOWSTONE_CAMPSITE_AVAILABILITY: str = f"{CAMPSITE_AVAILABILITY}/{YELLOWSTONE_PARK_PATH}"
    YELLOWSTONE_PROPERTY_INFO: str = f"{API_BASE_PATH}/property/rooms/{YELLOWSTONE_PARK_PATH}"
```

## Code after `black`

```python
class YellowstoneConfig:
    """
    Yellowstone Variable Storage Class
    """

    API_BASE_PATH: str = "v1/api"
    YELLOWSTONE_PARK_PATH: str = "yellowstonenationalparklodges"
    CAMPSITE_AVAILABILITY: str = f"{API_BASE_PATH}/availability/rooms"
    YELLOWSTONE_CAMPSITE_AVAILABILITY: str = (
        f"{CAMPSITE_AVAILABILITY}/{YELLOWSTONE_PARK_PATH}"
    )
    YELLOWSTONE_PROPERTY_INFO: str = (
        f"{API_BASE_PATH}/property/rooms/{YELLOWSTONE_PARK_PATH}"
    )
```

## Code After `ruff format`

Note that both the `original` and the `black` code will be changed to this

```python
class YellowstoneConfig:
    """
    Yellowstone Variable Storage Class
    """

    API_BASE_PATH: str = "v1/api"
    YELLOWSTONE_PARK_PATH: str = "yellowstonenationalparklodges"
    CAMPSITE_AVAILABILITY: str = f"{API_BASE_PATH}/availability/rooms"
    YELLOWSTONE_CAMPSITE_AVAILABILITY: (
        str
    ) = f"{CAMPSITE_AVAILABILITY}/{YELLOWSTONE_PARK_PATH}"
    YELLOWSTONE_PROPERTY_INFO: (
        str
    ) = f"{API_BASE_PATH}/property/rooms/{YELLOWSTONE_PARK_PATH}"
```

## Settings

- `ruff==0.1.3`
- `black==23.3.0`
- Settings: None

---

_Label `formatter` added by @MichaReiser on 2023-10-27 02:24_

---

_Comment by @MichaReiser on 2023-10-27 02:25_

Hey @juftin 

I think https://github.com/astral-sh/ruff/pull/8233 should have fixed this and is part of the latest ruff version 0.1.3 ([playground](https://play.ruff.rs/d5d3a42f-4664-4671-a58d-3ff443b03423)). 

Can you run `ruff version` and share the output with us? I'm trying to understand what's going on. 

---

_Comment by @juftin on 2023-10-27 02:32_

🤦 yep, you're right @MichaReiser. I was using my global ruff which was at `0.0.292` instead of my project's ruff (`0.1.3`). Everything looks perfect now. Really fantastic work. 

---

_Closed by @juftin on 2023-10-27 02:32_

---

_Comment by @MichaReiser on 2023-10-27 02:42_

Perfect. Thanks for reporting back. 

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-10-30 00:47_

---
