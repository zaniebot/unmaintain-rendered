```yaml
number: 1661
title: "rename doesn't work for imports from third-party files"
type: issue
state: closed
author: Gankra
labels:
  - server
assignees: []
created_at: 2025-11-28T02:27:24Z
updated_at: 2025-12-05T18:12:14Z
url: https://github.com/astral-sh/ty/issues/1661
synced_at: 2026-01-12T15:54:25Z
```

# rename doesn't work for imports from third-party files

---

_@Gankra_

### Summary

```py
import warnings as abc
from warnings import deprecated as xyz
x = abc
y = xyz
```

The two instances of `abc` and `xyz` are completely unaware of eachother, the lsp just doesn't understand Alias nodes at all. goto-references, find-references, and rename are all broken (weirdly not entirely consistently between vscode and sandbox).

goto-definition/declaration only works by virtue of resolving the aliases (i.e. not pointing at these symbols at all).

### Version

_No response_

---

_Label `server` added by @Gankra on 2025-11-28 02:27_

---

_Comment by @Gankra on 2025-11-28 02:29_

Once https://github.com/astral-sh/ruff/pull/21671 lands this will be basically a unique property of imports.

---

_Comment by @Gankra on 2025-11-28 02:35_

I haven't tracked down the precise issue but I think the issue vaguely amounts to `ide_support.rs` being quite confused about what the different Import* Definitions actually Mean.

---

_Comment by @Gankra on 2025-11-28 02:40_

<img width="367" height="145" alt="Image" src="https://github.com/user-attachments/assets/db1e2e33-2cfb-43b3-8fe5-87d62e3836bc" />

I'm also not a huge fan of the syntax highlighting on aliased symbols, especially because not all of the white-outted symbols are actually hidden! (Although this is something that has shifted in our semantics over time)

---

_Comment by @MichaReiser on 2025-12-01 11:25_

Not sure if related. 

```py
from pathlib import Path
from typing import Any, Final, override

import pytest
from lsprotocol import types as lsp

from benchmark.lsp_client import FileDiagnostics, LSPClient
from benchmark.projects import ALL as ALL_PROJECTS
from benchmark.projects import IncrementalEdit, Project
```

Find references works for `LSPClient` (first party file) but doesn't show any references for `Path` or `NamedTuple`. Finding references for `Final` works too

Interesting, this works in the [playground](https://play.ty.dev/76349f55-b1ef-4ef1-914c-c5fc51e4b23d) but not in Zed?

---

_Comment by @MichaReiser on 2025-12-01 11:55_

Oh interesting, this is a fairly spooky issue and I wonder what other issues this is causing. 

Go to definition on `Path` for the following example:

```py
from pathlib import Path

a = Path("test")
```

* Playground and in a project where find references works: Jumps to the definition in typeshed
* In `ty_benchmarks`: Jumps to `/opt/homebrew/Cellar/python@3.14/3.14.0_1/Frameworks/Python.framework/Versions/3.14/lib/python3.14/pathlib/__init__.py`, find references doesn't work

Go to declaration correct jumps to typeshed in both cases. 



---

_Comment by @MichaReiser on 2025-12-01 13:35_

The issue with `Path` is that `find_references` looks up the `definitions` of the symbol under the cursor, whereas the visitor resolves the declarations. We need to make sure that both places resolve the same targets.

@Gankra I'm not sure I understand the example from the issue summary. How are `abc` and `xyz` related to each other? 

---

_Comment by @Gankra on 2025-12-01 14:10_

It's just two separate examples I squished together, to show that both `import` and `from import` are independently broken (but likely for the same root cause). I worded it weird. What I meant is each `abc` doesn't see the other `abc`.

---

_Comment by @MichaReiser on 2025-12-01 17:01_

Finding references for `xyz` does work for me. It's only `abc` that returns an empty list of references (for reasons?)

---

_Comment by @MichaReiser on 2025-12-01 17:15_

The issue with `abc` is that we fail to resolve the `definition_targets` (it's an empty `Vec`) because it doesn't follow import aliases, which I think is correct. However `abc` also doesn't introduce its own definition (of `abc`)

---

_Comment by @Gankra on 2025-12-01 17:21_

> Finding references for xyz does work for me. It's only abc that returns an empty list of references (for reasons?)

Hmm I possibly regressed this when I landed my PR that simplified `Definitions` to a newtype instead of an enum.
Also notably I was seeing different behaviour in the playground vs vscode which was ???

---

_Closed by @MichaReiser on 2025-12-02 13:37_

---

_Comment by @Gankra on 2025-12-02 15:36_

While Micha made great improvements here, I'm reopening the issue because the *exact* case in my example still fails to `rename` because something gets freaked out and thinks we're trying to rename `warnings` itself and safety-blocks it.

(It works fine if the import is from a module in the current workspace)

---

_Reopened by @Gankra on 2025-12-02 15:36_

---

_Renamed from "find-references and rename doesn't work for imports" to "rename doesn't work for imports" by @MichaReiser on 2025-12-02 15:53_

---

_Renamed from "rename doesn't work for imports" to "rename doesn't work for imports from third-party files" by @MichaReiser on 2025-12-02 15:53_

---

_Closed by @MichaReiser on 2025-12-05 18:12_

---
