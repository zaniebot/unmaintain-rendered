```yaml
number: 7285
title: "Add import switching from `typing_extensions` to `typing` when reaches requisite version."
type: issue
state: closed
author: nstarman
labels:
  - needs-info
assignees: []
created_at: 2023-09-12T02:37:54Z
updated_at: 2023-09-12T12:41:07Z
url: https://github.com/astral-sh/ruff/issues/7285
synced_at: 2026-01-10T11:09:49Z
```

# Add import switching from `typing_extensions` to `typing` when reaches requisite version.

---

_Issue opened by @nstarman on 2023-09-12 02:37_

`typing_extensions` backports a lot of code from `typing`. It would be great if `ruff` could switch the import from `typing_extensions` to `typing` when ruff's target version matches (or exceeds) the requisite python version.

E.g.
```python
from typing_extensions import Annotated
```
Becomes 
```python
from typing import Annotated
```
when ruff's target version is 3.9 or greater.

This feels like a pyupgrade code?

---

_Comment by @charliermarsh on 2023-09-12 11:47_

I think this does exist as `UP035`: https://beta.ruff.rs/docs/rules/deprecated-import/

I tested it locally with `check foo.py --fix -n --select UP035 --target-version py39` and it did rewrite that import for me. Any luck?

---

_Label `waiting-on-author` added by @charliermarsh on 2023-09-12 11:47_

---

_Comment by @nstarman on 2023-09-12 12:35_

Yes, thanks! I was using an old test repo with target version set to py3.8. Updating that made everything work. Sorry for the noise!

---

_Closed by @MichaReiser on 2023-09-12 12:41_

---
