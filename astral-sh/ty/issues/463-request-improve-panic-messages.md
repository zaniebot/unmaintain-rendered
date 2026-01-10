```yaml
number: 463
title: "Request: Improve panic messages"
type: issue
state: closed
author: AA-Turner
labels:
  - diagnostics
  - wish
assignees: []
created_at: 2025-05-20T17:48:41Z
updated_at: 2025-08-15T12:12:58Z
url: https://github.com/astral-sh/ty/issues/463
synced_at: 2026-01-10T02:06:24Z
```

# Request: Improve panic messages

---

_Issue opened by @AA-Turner on 2025-05-20 17:48_

Minimal example of a panic (in this case duplicate of #256):

```python
from typing import TypeAlias
CircularList: TypeAlias = list[int | 'CircularList']
```

Panic output:

```ps1con
PS> ty -V
ty 0.0.1-alpha.5 (3b726d87a 2025-05-17)
PS> ty check panic.py
?[1;33mWARN?[0m ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
fatal[panic] Panicked at C:\Users\runneradmin\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\execute.rs:209:25 when checking `...\panic.py`: `infer_definition_types(Id(1401)): execute: too many cycle iterations`
Found 1 diagnostic
```

Currently there's a lot of clutter, including `C:\Users\runneradmin\.cargo\git\checkouts\`, which makes reading a fatal error line harder than it needs to be. Is it reasonably feasible to improve error messages for panics such as this?

Thanks for your work on `ty`, as always!

A


---

_Comment by @carljm on 2025-05-20 17:53_

I think we'd prefer to not have panics 😆 so not sure how much we'll prioritize this, but certainly not opposed.

---

_Label `diagnostics` added by @AlexWaygood on 2025-05-20 17:59_

---

_Label `wish` added by @AlexWaygood on 2025-05-20 17:59_

---

_Comment by @MichaReiser on 2025-05-20 18:08_

I'm not sure how much we can do here. The paths come from the rust backtrace. 

---

_Comment by @MichaReiser on 2025-08-15 12:12_

I'll close this. We're actively working on eliminating all panics, so that you hopefully won't ever see this message again :)

---

_Closed by @MichaReiser on 2025-08-15 12:12_

---
