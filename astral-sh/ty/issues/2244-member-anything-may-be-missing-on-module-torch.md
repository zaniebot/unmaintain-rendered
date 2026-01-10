```yaml
number: 2244
title: "Member <anything> may be missing on module `torch.distributed`"
type: issue
state: open
author: arogozhnikov
labels:
  - bug
  - imports
assignees: []
created_at: 2025-12-28T02:37:45Z
updated_at: 2025-12-29T17:15:36Z
url: https://github.com/astral-sh/ty/issues/2244
synced_at: 2026-01-10T01:56:41Z
```

# Member <anything> may be missing on module `torch.distributed`

---

_Issue opened by @arogozhnikov on 2025-12-28 02:37_

### Summary

```python
import torch.distributed as tdist

tdist.all_gather # complains on all ops I use: get_rank / get_world_size / ...
```

while this seemingly works

```py
from torch.distributed import ... 
```


### Version

ty 0.0.7

---

_Comment by @MichaReiser on 2025-12-29 11:12_

I'm not sure what the issue is exactly but all other type checkers that I tested (Pylance, Pyrefly) don't raise the same diagnostic.

---

_Label `bug` added by @MichaReiser on 2025-12-29 11:12_

---

_Label `imports` added by @MichaReiser on 2025-12-29 11:12_

---

_Comment by @carljm on 2025-12-29 16:43_

We have already disabled `possibly-unresolved-reference` and `possibly-missing-import` by default, due to the likelihood of false positives when libraries do complex conditional imports. (The latter being disabled by default is I think the only reason `from torch.distributed import ...` shows no issue here.) So for consistency I think we should also disable `possibly-missing-attribute` by default, otherwise you get this weird inconsistency where the import version is fine but the attribute version is not.

---

_Added to milestone `Pre-stable 1` by @carljm on 2025-12-29 16:43_

---

_Comment by @AlexWaygood on 2025-12-29 17:15_

> for consistency I think we should also disable `possibly-missing-attribute` by default, otherwise you get this weird inconsistency where the import version is fine but the attribute version is not.

We cannot do that without first tackling https://github.com/astral-sh/ty/issues/1656

---
