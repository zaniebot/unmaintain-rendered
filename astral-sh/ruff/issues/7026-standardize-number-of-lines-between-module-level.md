```yaml
number: 7026
title: Standardize number of lines between module-level docstring and first import
type: issue
state: closed
author: petermattia
labels: []
assignees: []
created_at: 2023-08-31T15:46:22Z
updated_at: 2023-09-06T16:09:18Z
url: https://github.com/astral-sh/ruff/issues/7026
synced_at: 2026-01-10T11:09:49Z
```

# Standardize number of lines between module-level docstring and first import

---

_Issue opened by @petermattia on 2023-08-31 15:46_

Currently, the below three snippets are all valid.

````python
"""My module."
import os
````

````python
"""My module."

import os
````

````python
"""My module."


import os
````

Is it possible to standardize to some convention? I don't have a preference for which format is best. In my experience, this is the only remaining nonstandardized import whitespace "issue".

Many thanks!



---

_Comment by @zanieb on 2023-08-31 18:50_

Would you expect this to be enforced by an existing lint rule, a new lint rule, or our formatter?

---

_Comment by @petermattia on 2023-08-31 19:39_

I don't see an existing lint rule for this. I don't have a strong expectation around how it would be implemented, although it seems like it could be a part of `ruff`'s `isort` functionality?

---

_Comment by @dhruvmanila on 2023-09-06 02:52_

Do you mean something like this #3759 ?

---

_Comment by @petermattia on 2023-09-06 16:09_

Yes exactly. I can close this issue since it's duplicated.

---

_Closed by @petermattia on 2023-09-06 16:09_

---
