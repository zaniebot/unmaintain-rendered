```yaml
number: 707
title: "Attribute \"Problem\" [unresolved-attribute]"
type: issue
state: closed
author: Julian-J-S
labels: []
assignees: []
created_at: 2025-06-26T08:51:11Z
updated_at: 2025-06-26T14:39:35Z
url: https://github.com/astral-sh/ty/issues/707
synced_at: 2026-01-12T15:54:23Z
```

# Attribute "Problem" [unresolved-attribute]

---

_@Julian-J-S_

I am getting a type error and I am not sure if its on `ty` or some fancy "hacks" in the library the `ty` does not understand (yet)  😆 

```python
from databricks.connect import DatabricksSession

spark = DatabricksSession.builder.getOrCreate()
```

## mypy & pyright

They think its fine ✅ 

## ty

```
error[unresolved-attribute]: Attribute `builder` can only be accessed on instances, not on the class object `<class 'DatabricksSession'>` itself.
 --> zzz.py:4:9
  |
4 | spark = DatabricksSession.builder.getOrCreate()
  |         ^^^^^^^^^^^^^^^^^^^^^^^^^
  |
info: rule `unresolved-attribute` is enabled by default
```

## pyrefly

```
ERROR Instance-only attribute `builder` of class `DatabricksSession` is not visible on the class [missing-attribute]
 --> /Users/juliansteden/work/databricks/dgdab/zzz.py:4:9
  |
4 | spark = DatabricksSession.builder.getOrCreate()
  |         ^^^^^^^^^^^^^^^^^^^^^^^^^
  |
```

## Pylance

seems to be fine with it, as I get normal autocomplete and also docstrings

<img width="526" alt="Image" src="https://github.com/user-attachments/assets/f2546c10-441e-4816-b5ff-bc475025f637" />

---

_Comment by @sharkdp on 2025-06-26 14:34_

This is how `DatabricksSession` declares the attribute:

```py
class DatabricksSession:
    # […]

    # This is a class property used to create a new instance of :class:DatabricksSession.Builder.
    # Autocompletion for VS Code breaks when usign the custom @classproperty decorator. Hence,
    # we initialize this later, by explicitly calling the classproperty decorator.
    builder: "DatabricksSession.Builder"
    """Create a new instance of :class:DatabricksSession.Builder"""


DatabricksSession.builder = classproperty(lambda cls: cls.Builder())
```

From ty's perspective, the `builder` attribute is not bound on the class, only declared. We currently consider these attribute to only be available on instances.

This is similar to https://github.com/astral-sh/ty/issues/384#issuecomment-2923413007 (see that specific comment for the planned resolution).

---

_Closed by @sharkdp on 2025-06-26 14:39_

---
