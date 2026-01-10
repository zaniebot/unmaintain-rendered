```yaml
number: 6294
title: "`useless-import-alias` (`PLC0414`) rule contradicts PEP484"
type: issue
state: closed
author: DetachHead
labels:
  - needs-decision
assignees: []
created_at: 2023-08-03T01:30:57Z
updated_at: 2025-06-20T18:20:28Z
url: https://github.com/astral-sh/ruff/issues/6294
synced_at: 2026-01-10T11:09:48Z
```

# `useless-import-alias` (`PLC0414`) rule contradicts PEP484

---

_Issue opened by @DetachHead on 2023-08-03 01:30_

```py
import numpy as numpy
```
this means explicit re-export, as mentioned in [PEP484](https://peps.python.org/pep-0484/):

> Modules and variables imported into the stub are not considered exported from the stub unless the import uses the import ... as ... form or the equivalent from ... import ... as ... form. (UPDATE: To clarify, the intention here is that only names imported using the form X as X will be exported, i.e. the name before and after as must be the same.)

perhaps this rule should be removed, since it contradicts type checking rules

---

_Comment by @qdegraaf on 2023-08-03 11:01_

This is only the case for stub files. Ruff takes this into account by only running the rule for non-stub files

See https://github.com/astral-sh/ruff/blob/9f3567dea6f04e9163ab66ad2fc1fafbadb3e790/crates/ruff/src/checkers/ast/analyze/statement.rs#L568-L572 and https://github.com/astral-sh/ruff/blob/9f3567dea6f04e9163ab66ad2fc1fafbadb3e790/crates/ruff/src/checkers/ast/analyze/statement.rs#L888-L892 

---

_Comment by @zanieb on 2023-08-03 15:01_

Hm per https://github.com/microsoft/pyright/blob/ada1ea63e8360fe3bd203bff3b0069c34d142817/docs/typed-libraries.md#library-interface this also applies to non-stub files. I think we've considered turning this rule off in `__init__.py` files.

---

_Comment by @charliermarsh on 2023-08-03 15:03_

I feel like this rule shouldn’t exist, those explicit aliases do have semantic meaning now, but it’s a bit strange to remove entirely since it does exist in pylint.

---

_Comment by @charliermarsh on 2023-08-03 23:51_

Some projects do have this explicitly enabled (home-assistant, for example).

---

_Label `needs-decision` added by @charliermarsh on 2023-08-03 23:51_

---

_Comment by @KotlinIsland on 2023-08-04 02:52_

> I feel like this rule shouldn’t exist

I agree

> Some projects do have this explicitly enabled (home-assistant, for example).

They use mypy, but they don't export their types. From the looks of it, they don't use any explicit reexports, perhaps they will at some point. If they do, then they would certainly remove this rule.

> but it’s a bit strange to remove entirely since it does exist in pylint.

So the rule was invented before this new semantic meaning was introduced, now it seems invalid.

Perhaps if a project were to forgo typing (or just doesn't like the concept of explicit reexport) then this rule would be applicable.


---

_Comment by @Avasam on 2024-03-25 19:44_

Ignoring this rule from `__init__.py` definitely makes sense to me.
Could still be considered a code smell (ie: did you really mean to re-export?) elsewhere. For instance in pywin32 I found a handful of old actual aliases that are no longer relevant after the Python 3 transition, but were accidentally left in.
Maybe such a distinction would make more sense after #1774 / #1256

In the meantime it can be configured as such:
```toml
[lint.per-file-ignores]
# Explicit re-exports is fine in __init__.py, still a code smell elsewhere.
"__init__.py" = ["PLC0414"]
```

---

_Comment by @Hawk777 on 2024-06-07 02:46_

I agree it would be good to ignore this in `__init__.py`. It contradicts F401; as far as I know there’s no non-`noqa` way to have both F401 and PLC0414 enabled and re-export things from an `__init__.py` right now, which is quite unfortunate.

---

_Comment by @alicederyn on 2025-05-07 16:35_

@Hawk777 Can you not use `__all__`? e.g.
```python
from ._inner_module import thing_to_reexport
__all__ = ["thing_to_reexport"]
```
(Though I fully agree with the proposed fix to PLC0414.)

---

_Comment by @Hawk777 on 2025-05-09 16:04_

@alicederyn Good point; I didn’t try that and unfortunately now, almost a year later, I don’t remember which of my projects I ran into this problem in 😆  It may well have worked though.

---

_Closed by @dylwil3 on 2025-06-20 18:20_

---
