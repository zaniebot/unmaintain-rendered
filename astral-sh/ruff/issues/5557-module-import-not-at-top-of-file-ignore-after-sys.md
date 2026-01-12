```yaml
number: 5557
title: "`module-import-not-at-top-of-file` ignore after `sys.path.insert`"
type: issue
state: closed
author: tgross35
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-07-06T09:20:01Z
updated_at: 2023-12-07T18:35:56Z
url: https://github.com/astral-sh/ruff/issues/5557
synced_at: 2026-01-12T15:54:45Z
```

# `module-import-not-at-top-of-file` ignore after `sys.path.insert`

---

_@tgross35_

Sometimes you need to insert a path to be able to import something that's out of scope from your current file - in which case, the imports must come after the `insert` statement. For example, from Rust's repo:

```py
from __future__ import absolute_import, division, print_function
import os
import unittest
# ...

bootstrap_dir = os.path.dirname(os.path.abspath(__file__))
sys.path.insert(0, bootstrap_dir)
import bootstrap # noqa: E402
import configure # noqa: E402
```

My suggestion is to ignore E402 `module-import-not-at-top-of-file` if it comes after a `sys.path.insert` statement.

There are legitimate uses of `sys.path.insert`, but it could be considered a hack or pitfall for new users - so I could understand potentially not wanting this behavior as the default. If this is the case, I would propose one of the following:

- Create a configuration option such as `ignore-after-sys-path-insert`
- Do this change as suggested, but create a separate lint that checks for `sys.path.insert` usage. The lint could theoretically be kind of smart and suggest possible alternatives for common misuses (adding `__init__.py`, importing directly, etc) but that kind of gets into evaluating the context

---

_Label `question` added by @charliermarsh on 2023-07-09 20:02_

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:06_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:06_

---

_Label `rule` added by @charliermarsh on 2023-07-10 01:06_

---

_Closed by @charliermarsh on 2023-12-07 18:35_

---
