```yaml
number: 1107
title: "[feature-request] Require first-party imports to target highest exporting non-ancestor module"
type: issue
state: open
author: smackesey
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2022-12-06T12:23:06Z
updated_at: 2023-12-04T22:26:14Z
url: https://github.com/astral-sh/ruff/issues/1107
synced_at: 2026-01-10T11:09:43Z
```

# [feature-request] Require first-party imports to target highest exporting non-ancestor module

---

_Issue opened by @smackesey on 2022-12-06 12:23_

A symbol can be defined in one module and re-exported from several others. When importing a symbol from an external typed library, the convention is to import from as close to the root as possible. This is how popular auto-import resolution tools work (see my very similar post in [this pylance discussion](https://github.com/microsoft/pylance-release/discussions/3050#discussioncomment-3197660): 

>If the same name is exported from multiple modules, the auto-import logic in pyright and pylance prefer the shortest module path. Only the shortest path is listed in the completion list, and the longer paths are de-duped.

In large projects, sometimes you have big submodules that you want to provide an "internal public" interface for-- e.g. if you have a large submodule `project.foo`, you might want project-internal code to import only from `project.foo` itself, rather than `project.foo.*` submodules.

I'd like to suggest that `ruff` offer a rule that enforces this by requiring internal imports resolve to the shortest module that exposes a symbol. So:

```
### project/foo/__init__.py
# redundant alias is necessary to expose `BAR` as public for `project.foo`
from .bar import BAR as BAR

### project/foo/bar.py
BAR = "BAR"

### project/baz.py
from project.foo.bar import BAR   # ERROR: should import from project.foo instead
```

---

_Renamed from "[feature-request] Require first-party imports to target highest exporting sibling-or-lower exporting module" to "[feature-request] Require first-party imports to target highest exporting non-ancestor module" by @smackesey on 2022-12-06 12:23_

---

_Comment by @charliermarsh on 2022-12-09 04:50_

Yeah this makes sense. It would also force us to support cross-module checks (since we'd need to build a mapping from module to exported symbols), which would be a good thing.

---

_Label `rule` added by @charliermarsh on 2022-12-09 04:50_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:31_

---

_Comment by @chbndrhnns on 2023-11-09 17:07_

I sometimes see modules prefixed with an underscore being treated as private. Here is an example how this could work:

```py
# ./client.py
from .myp._private import Member
from .myp._private import OtherMember

assert Member

# ./myp/__init__.py
__all__ = ["Member"]

from ._private import Member

# ./myp/_private.py
class Member: ...

class OtherMember: ...
```

Ruff could turn this into:

```py
# ./client.py
from .myp import Member
from .myp._private import OtherMember

assert Member

# ./myp/__init__.py
__all__ = ["Member"]

from ._private import Member

# ./myp/_private.py
class Member: ...

class OtherMember: ...
```

---

_Comment by @smackesey on 2023-12-04 22:26_

Just wondering whether this is now easily doable with all the changes in the last year to ruff. Would be a nice QOL improvement for us.

---
