```yaml
number: 1822
title: Promote literals in unannotated module globals
type: issue
state: open
author: JanFontanet
labels: []
assignees: []
created_at: 2025-12-09T08:24:58Z
updated_at: 2025-12-24T13:04:04Z
url: https://github.com/astral-sh/ty/issues/1822
synced_at: 2026-01-10T01:56:41Z
```

# Promote literals in unannotated module globals

---

_Issue opened by @JanFontanet on 2025-12-09 08:24_

### Summary

I've a 3rd party library that exposes some configuration through module variables, so I've to overwrite them in order for the library to work as I want. But when I run `ty check` on my code I got an error that can be reproduced with the minimal example below.

a.py:
```python
MY_VAR=1
```
main.py
```python
import a

a.MY_VAR=2
```

I got the following error:
```
Object of type `Literal[2]` is not assignable to attribute `MY_VAR` of type `Literal[1]` (invalid-assignment) [Ln 3, Col 1]
```



### Version

ty 0.0.1-alpha.32

---

_Added to milestone `Stable` by @carljm on 2025-12-09 18:50_

---

_Comment by @carljm on 2025-12-09 18:50_

Thanks for the report!

We probably do need to do literal-promotion on un-annotated module globals, now that we aren't unioning them with `Unknown`.

---

_Renamed from "Module variables inferred as literals" to "Promote literals in unannotated module globals" by @carljm on 2025-12-09 18:51_

---

_Comment by @AlexWaygood on 2025-12-24 13:04_

There was a lot of previous discussion around this in @sharkdp's PRs https://github.com/astral-sh/ruff/pull/20643 and https://github.com/astral-sh/ruff/pull/20664. I support experimenting with this, but I think there are definitely downsides as well as upsides to doing this.

---
