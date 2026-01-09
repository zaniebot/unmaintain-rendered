---
number: 15643
title: "__import__(\"pdb\") doesn't trigger error T100"
type: issue
state: open
author: BtPht
labels:
  - core
assignees: []
created_at: 2025-01-21T14:30:54Z
updated_at: 2025-01-21T15:41:07Z
url: https://github.com/astral-sh/ruff/issues/15643
synced_at: 2026-01-07T13:12:16-06:00
---

# __import__("pdb") doesn't trigger error T100

---

_Issue opened by @BtPht on 2025-01-21 14:30_

The error [T100](https://docs.astral.sh/ruff/rules/debugger/) is correctly triggered when pdb is imported with `import pdb` but not when the dunder method `__import__` is used.

```shell
➜  ruff git:(main) ✗ cat import_pdb.py 
import pdb

pdb.set_trace()
➜  ruff git:(main) ✗ cargo run -p ruff -- check --select T100 import_pdb.py --no-cache 
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.18s
     Running `target/debug/ruff check --select T100 import_pdb.py --no-cache`
import_pdb.py:1:1: T100 Import for `pdb` found
  |
1 | import pdb
  | ^^^^^^^^^^ T100
2 |
3 | pdb.set_trace()
  |

import_pdb.py:3:1: T100 Trace found: `pdb.set_trace` used
  |
1 | import pdb
2 |
3 | pdb.set_trace()
  | ^^^^^^^^^^^^^^^ T100
  |

Found 2 errors.
```

And the not detected import :
```shell
➜  ruff git:(main) ✗ cat dunderimport_pdb.py 
__import__("pdb").set_trace()

➜  ruff git:(main) ✗ cargo run -p ruff -- check --select T100 dunderimport_pdb.py --no-cache 
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.18s
     Running `target/debug/ruff check --select T100 dunderimport_pdb.py --no-cache`
All checks passed!
```

---

_Comment by @MichaReiser on 2025-01-21 15:09_

Thanks. 

I don't think Ruff supports any of the dynamic import mechanisms (`__import__`, `import_module`, `importlib.import_module` etc.). So it sort of makes sense that it misses those calls. We could consider supporting them in the future, although I consider it lower priority because `__import__` et al are considered more advanced use cases.

---

_Label `core` added by @MichaReiser on 2025-01-21 15:09_

---

_Comment by @BtPht on 2025-01-21 15:41_

Understood.
For reference this unconventional import comes from [friendly-snippets](https://github.com/rafamadriz/friendly-snippets/blob/main/snippets/python/debug.json#L4) so it might catch some people by surprise.

---
