---
number: 6288
title: Collapse short multi-line import groups to one line with I001
type: issue
state: closed
author: andersk
labels: []
assignees: []
created_at: 2023-08-02T22:13:58Z
updated_at: 2023-08-03T23:55:02Z
url: https://github.com/astral-sh/ruff/issues/6288
synced_at: 2026-01-07T13:12:15-06:00
---

# Collapse short multi-line import groups to one line with I001

---

_Issue opened by @andersk on 2023-08-02 22:13_

Ruff, like upstream isort, expands single-line import groups that are too long to multiple lines.

```diff
-from typing import Any, Callable, Dict, List, Mapping, Optional, Sequence, Tuple, TypeVar
+from typing import (
+    Any,
+    Callable,
+    Dict,
+    List,
+    Mapping,
+    Optional,
+    Sequence,
+    Tuple,
+    TypeVar,
+)
```

Upstream isort, but not Ruff, also does the reverse: it collapses multi-line import groups that are short enough to a single line. I think we should do this too.

```diff
-from typing import (
-    Any,
-    Callable,
-    Dict,
-    List,
-    Mapping,
-    Optional,
-    Sequence,
-    Tuple,
-)
+from typing import Any, Callable, Dict, List, Mapping, Optional, Sequence, Tuple
```

---

_Comment by @charliermarsh on 2023-08-03 00:04_

I believe we collapse such imports if you disable [`split-on-trailing-comma`](https://beta.ruff.rs/docs/settings/#isort-split-on-trailing-comma).

I've looked into this before, and [isort's documentation](https://pycqa.github.io/isort/docs/configuration/profiles.html) states that this setting is enabled for `profile = "black"`, but in practice it appears not to be.

---

_Comment by @andersk on 2023-08-03 00:07_

It works for me (isort 5.12.0).

```console
$ cat > a.py
from typing import (
    Any,
    Callable,
)
$ isort --profile=black a.py
Fixing /tmp/a.py
$ cat a.py
from typing import Any, Callable
```

---

_Comment by @charliermarsh on 2023-08-03 00:09_

What I meant is that: the isort documentation suggests that it _shouldn't_ be removing magic trailing commas there. The documentation says that `split-on-trailing-comma: true` is the default for that profile. But in practice it appears not to be.

---

_Comment by @charliermarsh on 2023-08-03 00:11_

To be clear: we support this behavior, it's just not our default.

---

_Comment by @andersk on 2023-08-03 00:11_

Got it. Feel free to close if you think this should remain as is.

I guess this is a duplicate of

- #4153

---

_Comment by @charliermarsh on 2023-08-03 23:55_

I'm torn on it but I'm going to leave as-is for now.

---

_Closed by @charliermarsh on 2023-08-03 23:55_

---
