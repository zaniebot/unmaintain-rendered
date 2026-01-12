```yaml
number: 1275
title: Missing-Argument Rule Does Not Support Collection Unpacking Notation for Function Parameters
type: issue
state: closed
author: aleexharris
labels: []
assignees: []
created_at: 2025-09-29T13:10:10Z
updated_at: 2025-09-29T13:24:34Z
url: https://github.com/astral-sh/ty/issues/1275
synced_at: 2026-01-12T15:54:24Z
```

# Missing-Argument Rule Does Not Support Collection Unpacking Notation for Function Parameters

---

_@aleexharris_

### Summary

For obvious reasons, the following useful pattern causes issues with ty:
```py
@dataclass
class Config:
    a: str
    b: bool
    c: float

    def from_file(config_path: str):
        with open(config_path, "r") as f:
            cfg = json.load(f)
            return Config(**cfg)
```

I tried to get around this by using object hooks and typed dicts, but I still get the same issues.
```py
ConfigDict = TypedDict(
    "ConfigDict",
    {
        "a": str,
        "b": bool,
        "c": float,
    },
)


@dataclass
class Config:
    a: str
    b: bool
    c: float

    def from_file(config_path: str):
        def object_hook(dct: ConfigDict) -> Config:
            return Config(**dct)

        with open(config_path, "r") as f:
            return json.load(f, object_hook=object_hook)
```

That still didn't work. The error shown in both cases is this:
```bash
error[missing-argument]: No arguments provided for required parameters `a`, `b`, `c`
  --> src/config.py:83:20
   |
81 |         with open(config_path, "r") as f:
82 |             cfg = json.load(f)
83 |             return Config(**cfg)
   |                    ^^^^^^^^^^^^^
info: rule `missing-argument` is enabled by default

```

Please add support for collection unpacking using ** notation and/or highlight a suggested fix. Thanks.

### Version

ty 0.0.1-alpha.21

---

_Renamed from "Support Collection Unpacking Notation for Function Parameters" to "Missing-Argument Rule Does Not Support Collection Unpacking Notation for Function Parameters" by @aleexharris on 2025-09-29 13:22_

---

_Comment by @sharkdp on 2025-09-29 13:24_

Thank you for reporting this, please see #247 

---

_Closed by @sharkdp on 2025-09-29 13:24_

---
