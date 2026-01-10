```yaml
number: 1434
title: Goto definition on import statements follows submodules inconsistently?
type: issue
state: closed
author: saucoide
labels:
  - server
assignees: []
created_at: 2025-10-24T10:54:30Z
updated_at: 2025-10-24T23:01:06Z
url: https://github.com/astral-sh/ty/issues/1434
synced_at: 2026-01-10T02:06:25Z
```

# Goto definition on import statements follows submodules inconsistently?

---

_Issue opened by @saucoide on 2025-10-24 10:54_

### Summary


I couldn't find a similar issue, and maybe i'm understanding this wrong, but
when using ty as an lsp, this surprises me:


```python
# main.py
from foo import bar               # cursor on `bar` -> goto definition -> nothing
import foo.bar as bar             # cursor on `bar` -> goto definition -> takes you to bar.py
from foo.bar import Barista       # cursor on `bar` -> goto definition -> takes you to bar.py
```

I'm not sure why in the first statement it does not take you to the submodule


this is with a structure like this:

```
├──  foo
│   ├──  __init__.py
│   ├──  bar.py
│   └──  main.py
├──  pyproject.toml

```

`__init__.py` is empty & `bar.py` just has a dummy class definition


### Version

ty 0.0.1-alpha.23 (5c51b8480 2025-10-16)



edit: 

easier to test with an example in the stdlib

```python
from json import decoder
import json.decoder as decoder
from json.decoder import JSONDecodeError
```

---

_Label `server` added by @AlexWaygood on 2025-10-24 10:55_

---

_Comment by @MichaReiser on 2025-10-24 15:55_

Thank you. I think this is the same as https://github.com/astral-sh/ty/issues/1011

---

_Closed by @MichaReiser on 2025-10-24 15:55_

---

_Comment by @saucoide on 2025-10-24 23:01_

thanks, will follow that one

---
