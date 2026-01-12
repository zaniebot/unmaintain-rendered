```yaml
number: 3064
title: "fix(isort): comply with isort when sorting sub-package modules"
type: pull_request
state: closed
author: spaceone
labels: []
assignees: []
draft: true
base: main
head: 3059-isort-package-sorting
created_at: 2023-02-20T18:26:19Z
updated_at: 2023-03-21T15:07:25Z
url: https://github.com/astral-sh/ruff/pull/3064
synced_at: 2026-01-12T04:39:44Z
```

# fix(isort): comply with isort when sorting sub-package modules

---

_Pull request opened by @spaceone on 2023-02-20 18:26_

e.g. packages named "foo.bar"

additionally make sure `__future__` imports are always sorted at the very top.

Fixes #3059

---

_Converted to draft by @spaceone on 2023-02-20 18:49_

---

_Marked ready for review by @spaceone on 2023-02-20 19:40_

---

_@charliermarsh reviewed on 2023-02-20 20:13_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/isort/categorize.rs`:64 on 2023-02-20 20:13_

What does isort do if you have:

```
known-first-party = ["foo"]
known-third-party = ["foo.bar"]
```

And then you import `foo.bar.baz`. Does that get classified as first- or third-party?

---

_@spaceone reviewed on 2023-02-21 09:50_

---

_Review comment by @spaceone on `crates/ruff/src/rules/isort/categorize.rs`:64 on 2023-02-21 09:50_

```ini
[settings]
known_first_party=foo
known_third_party=foo.bar
```
results in:
```python
import sys

import foo.bar
import foo.bar.baz
from foo.bar import blah, blub
from foo.bar.baz import something

import foo
from foo import bar, baz
```

So my MR currently has this diff:
```diff
ruff --select I --fix --diff foo.py 
--- foo.py
+++ foo.py
@@ -1,9 +1,9 @@
 import sys
 
 import foo.bar
-import foo.bar.baz
 from foo.bar import blah, blub
-from foo.bar.baz import something
 
 import foo
+import foo.bar.baz
 from foo import bar, baz
+from foo.bar.baz import something

Would fix 1 error.
```


---

_Converted to draft by @spaceone on 2023-02-22 17:50_

---

_Comment by @charliermarsh on 2023-03-21 15:07_

I'm going to close this for now, just to keep the open PR list tight. If you choose to return to this, we can always reopen!

---

_Closed by @charliermarsh on 2023-03-21 15:07_

---
