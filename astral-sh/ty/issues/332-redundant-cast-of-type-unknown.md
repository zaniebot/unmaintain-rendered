```yaml
number: 332
title: redundant-cast of type Unknown
type: issue
state: closed
author: tonka3000
labels:
  - needs-info
assignees: []
created_at: 2025-05-12T18:46:46Z
updated_at: 2025-05-13T16:35:34Z
url: https://github.com/astral-sh/ty/issues/332
synced_at: 2026-01-12T15:54:22Z
```

# redundant-cast of type Unknown

---

_@tonka3000_

### Summary

I have the following code. `getFactory` is a C++ binded function and has no typing, so the return type is `Unknown`.
ty detect it as redundant cast which is not correct in my opinion. 

Expected behaviour would be the the result of the cast is `LDSG3DMeasurementFactory` type without a warning.

```sh
warning[redundant-cast]: Value is already of type `Unknown`
  --> lds_geometry\mech\measurements.py:21:16
   |
20 |         fac_man = self.doc.getFactoryManager()
21 |         return cast(LDSG3DMeasurementFactory, fac_man.getFactory("LDSG3DMeasurementFactory"))
```

### Version

ty 0.0.0-alpha.8 (0474b40e1 2025-05-09)

---

_Comment by @AlexWaygood on 2025-05-12 19:01_

Thanks for the report. We'll need some more information to be able to track down why we incorrectly think the type of the `LDSG3DMeasurementFactory` symbol in your file here is `Unknown`. Can you show us how that is defined in `lds_geometry\mech\measurements.py`? If it's defined through an import in `lds_geometry\mech\measurements.py`, we may need to see its original definition in the module it's defined in as well.

---

_Label `needs-info` added by @carljm on 2025-05-12 22:22_

---

_Comment by @tonka3000 on 2025-05-13 13:47_

Hi,

I checked it in more detail. It seems to be a general thing when pyi files are involved.
I use PythonQt (mevis lab) for my bindings and I have not the typical python interpreter file structure.
The general problem is that both side are `Unknown` which explains why the `redundant_cast` is emitted.

In general I apply the `--extra-search-path` on the terminal to the internal `site-packages` folder of the bundled interpreter which includes the pyi files. I assume that the pyi files are not discovered and therefore the types are `Unknown`.
I have the same behavior in the ty VSCode extension and I could not find an option for extra-paths like Pylance/Pyright have (`python.autoComplete.extraPaths`).

Pylance/Pyright works normally with the same information.

My structure looks like follow on Windows:

- DLLs
- lib
- site-packages
  - PythonQt
    - LDSCore.pyi
    - LDSPLGeom3D.pyi

`--extra-search-path` point to site-packages folder (absolute path).

Not quite sure if there are any flags or missing files that ty would need to read in the pyi files.

I reduce it to the example below.

```py
from PythonQt.LDSCore import LDSDoc
from PythonQt.LDSPLGeom3D import LDSGeom3DDoc # LDSGeom3DDoc is Unknown

class FooClass:
  def __init__(self, doc: LDSDoc):
    super().__init__()
    self.doc = doc

  def foo(self):
    geo_doc = cast(LDSGeom3DDoc, self.doc) # Here the cast is cast(Unknown, Unknown) when pyi files are not read
```

btw: The code is closed source, I would need to send you that via mail or another communication channel if you see it as required step.

---

_Comment by @tonka3000 on 2025-05-13 16:32_

Seems to be fixed since `0.0.1-alpha.1 (12f466e46 2025-05-13)`

---

_Closed by @AlexWaygood on 2025-05-13 16:35_

---
