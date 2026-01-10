```yaml
number: 1654
title: "`docker.types` reported as `possibly-missing-attribute` after `import docker; from docker.types import Ulimit`"
type: issue
state: open
author: sitsofe
labels:
  - imports
assignees: []
created_at: 2025-11-27T10:45:25Z
updated_at: 2025-12-18T12:01:22Z
url: https://github.com/astral-sh/ty/issues/1654
synced_at: 2026-01-10T01:53:59Z
```

# `docker.types` reported as `possibly-missing-attribute` after `import docker; from docker.types import Ulimit`

---

_Issue opened by @sitsofe on 2025-11-27 10:45_

### Question

Hello,

I have the following `test.py`:
```python
import docker
from docker.types import Ulimit

docker.types.Ulimit(name='nproc', soft=1024)
Ulimit(name='nproc', soft=1024)
```

and I have `docker==7.1.0` installed in an activte venv.

When I run `ty` (ty 0.0.1-alpha.28 (8c342496a 2025-11-25)) I get:
```
> ty check test.py
warning[possibly-missing-attribute]: Submodule `types` may not be available as an attribute on module `docker`
 --> test.py:4:1
  |
2 | from docker.types import Ulimit
3 |
4 | docker.types.Ulimit(name='nproc', soft=1024)
  | ^^^^^^^^^^^^
5 | Ulimit(name='nproc', soft=1024)
  |
help: Consider explicitly importing `docker.types`
info: rule `possibly-missing-attribute` is enabled by default

Found 1 diagnostic
```

pyright (pyright 1.1.407) also complains:
```
pyright test.py
/private/tmp/q/test.py
  /private/tmp/q/test.py:4:8 - error: "types" is not a known attribute of module "docker" (reportAttributeAccessIssue)
```

After installing stubs, mypy (mypy 1.18.2 (compiled: yes)) doesn't complain:
```
> mypy test.py
Success: no issues found in 1 source file
```

python (Python 3.12.11 on macOS) itself doesn't complain:
```
> python test.py 
>
```

Looking at https://github.com/docker/docker-py/blob/7.1.0/docker/__init__.py there is no import that create `types` but there is an adjacent `types/` directory that contains https://github.com/docker/docker-py/blob/7.1.0/docker/types/__init__.py and that imports `Ulimit`.

I have done searches https://github.com/astral-sh/ty/issues?q=unresolved-attribute and https://github.com/astral-sh/ty/issues?q=possibly-missing-attribute but I can't find an existing duplicate but I'm sure one must already exist. Can you point me in the right direction?

### Version

ty 0.0.1-alpha.28 (8c342496a 2025-11-25)

---

_Label `question` added by @sitsofe on 2025-11-27 10:45_

---

_Comment by @AlexWaygood on 2025-11-27 10:55_

We don't have an issue for this currently, but this would be fixed by https://github.com/astral-sh/ruff/pull/21583, which is a joint effort by me and @Gankra :-)

---

_Label `imports` added by @AlexWaygood on 2025-11-27 10:55_

---

_Renamed from "Which existing issue covers "Missing attribute error" behaviour when using docker submodule?" to "`docker.types` reported as `possibly-missing-attribute` after `import docker; from dockter.types import Ulimit`" by @AlexWaygood on 2025-11-27 10:57_

---

_Label `question` removed by @AlexWaygood on 2025-11-27 10:57_

---

_Renamed from "`docker.types` reported as `possibly-missing-attribute` after `import docker; from dockter.types import Ulimit`" to "`docker.types` reported as `possibly-missing-attribute` after `import docker; from docker.types import Ulimit`" by @sitsofe on 2025-11-27 14:56_

---

_Comment by @sitsofe on 2025-11-27 14:57_

@AlexWaygood Ah a PR is even better! Thanks for pointing me towards it.

---

_Added to milestone `Stable` by @carljm on 2025-12-02 03:13_

---

_Comment by @spaceone on 2025-12-18 12:01_

This issue is probably the most common one found in my projects.



---
