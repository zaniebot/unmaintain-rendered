```yaml
number: 373
title: "error[invalid-type-form]: Variable of type `<module 'PIL.Image'>` is not allowed in a type expression"
type: issue
state: closed
author: Ryang20718
labels:
  - diagnostics
assignees: []
created_at: 2025-05-13T23:44:22Z
updated_at: 2025-05-21T19:38:57Z
url: https://github.com/astral-sh/ty/issues/373
synced_at: 2026-01-10T02:34:09Z
```

# error[invalid-type-form]: Variable of type `<module 'PIL.Image'>` is not allowed in a type expression

---

_Issue opened by @Ryang20718 on 2025-05-13 23:44_

### Summary

```
from PIL import Image


def hello(x: Image):
     print(x)
```

error[invalid-type-form]: Variable of type `<module 'PIL.Image'>` is not allowed in a type expression

### Version

ty 0.0.0-alpha.8

---

_Comment by @AlexWaygood on 2025-05-13 23:52_

Thanks for the report. However, I think ty is correct in emitting this diagnostic. Mypy also emits a diagnostic on this code -- here's what it says here:

```
% uvx --with=Pillow mypy foo.py      
Installed 4 packages in 11ms
foo.py:3: error: Module "PIL.Image" is not valid as a type  [valid-type]
foo.py:3: note: Perhaps you meant to use a protocol matching the module structure?
Found 1 error in 1 file (checked 1 source file)
```

And so does pyright:

```
% uvx --with=Pillow pyright foo.py
Installed 4 packages in 91ms
/Users/alexw/dev/ruff/foo.py
  /Users/alexw/dev/ruff/foo.py:3:10 - error: Module cannot be used as a type (reportGeneralTypeIssues)
1 error, 0 warnings, 0 informations
```

Do you find our diagnostic here confusing? Could we make it clearer? :-)

---

_Label `needs-info` added by @AlexWaygood on 2025-05-13 23:54_

---

_Label `question` added by @AlexWaygood on 2025-05-13 23:54_

---

_Comment by @jelle-openai on 2025-05-14 00:14_

You probably want to use `Image.Image` as an annotation, for what it's worth. `PIL.Image` is a module, `PIL.Image.Image` is a class.

Possibly ty could detect this case (a module having a member with the same name) and suggest that that's what the user meant.

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-05-21 17:07_

---

_Label `question` removed by @AlexWaygood on 2025-05-21 17:07_

---

_Label `needs-info` removed by @AlexWaygood on 2025-05-21 17:07_

---

_Label `diagnostics` added by @AlexWaygood on 2025-05-21 17:07_

---

_Closed by @AlexWaygood on 2025-05-21 19:38_

---
