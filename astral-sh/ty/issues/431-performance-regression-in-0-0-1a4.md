```yaml
number: 431
title: Performance regression in 0.0.1a4
type: issue
state: closed
author: danielhollas
labels:
  - performance
assignees: []
created_at: 2025-05-16T21:52:27Z
updated_at: 2025-05-17T12:39:14Z
url: https://github.com/astral-sh/ty/issues/431
synced_at: 2026-01-10T02:34:10Z
```

# Performance regression in 0.0.1a4

---

_Issue opened by @danielhollas on 2025-05-16 21:52_

### Summary

I noticed a big performance regression in the latest release. Ultimately (and to my surprise) I was able to produce the following very simple reproducer:

```python
from pathlib import Path

def func(p: Path) -> None:
     p.read_bytes()
```

```console
/tmp via C v13.3.1-gcc via 🐍 v3.12.7 
❯ time uvx ty@0.0.1a4 check test.py 
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
All checks passed!

real	0m0.891s
user	0m0.723s
sys	0m0.165s
/tmp via C v13.3.1-gcc via 🐍 v3.12.7 
❯ time uvx ty@0.0.1a3 check test.py 
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
All checks passed!

real	0m0.070s
user	0m0.025s
sys	0m0.033s
```

I am honestly still thinking that I clearly must be doing something wrong... 😰

I have bisected the issue to this PR: https://github.com/astral-sh/ruff/pull/18010
(this is my first time bisecting a rust program so I might have messed up somewhere).

Running with debug output reveales that the execution "freezes" always at this output:

```
  2025-05-16T21:50:37.723099Z  INFO ty_project: Indexed 1 file(s)
    at crates/ty_project/src/lib.rs:421 on ThreadId(2)
    in ty_project::Project::index_files with project=tmp
    in ty_project::Project::check

Checking ------------------------------------------------------------ 0/1 files                                                             2025-05-16T21:50:37.723229Z DEBUG ty_python_semantic::types: Checking file '/tmp/test.py'
    at crates/ty_python_semantic/src/types.rs:88 on ThreadId(2)
    in ty_project::check_file with file=file(Id(800))
    in ty_project::Project::check
```

But the only thing after that is
```
All checks passed!
  2025-05-16T21:50:38.568862Z DEBUG ty: Exiting main loop
    at crates/ty/src/lib.rs:227 on ThreadId(1)
```

### Version

0.0.1a4

---

_Label `performance` added by @AlexWaygood on 2025-05-16 21:56_

---

_Comment by @danielhollas on 2025-05-16 22:01_

Hmm, `tomllib` does not depend on `pathlib` so it is at least conceivable that this was missed in Codspeed benchmarks? I also don't see any 800ms sleep added anywhere in https://github.com/astral-sh/ruff/pull/18010? 😅 

---

_Comment by @danielhollas on 2025-05-17 00:18_

I added the code snippet from above to the vendored tomllib package that's used to benchmark ty   and indeed Codspeed is **not** happy. 

https://github.com/astral-sh/ruff/pull/18145#issuecomment-2887873362

---

_Comment by @carljm on 2025-05-17 11:42_

Thanks for reporting and bisecting this! Taking a look.

---

_Closed by @carljm on 2025-05-17 12:27_

---

_Comment by @carljm on 2025-05-17 12:36_

Reverted the offending change for now so we can make a fixed release; will dig further into the cause of the regression later.

---

_Comment by @carljm on 2025-05-17 12:39_

(Once we identify the root cause, we should also add a benchmark which would have caught this.)

---
