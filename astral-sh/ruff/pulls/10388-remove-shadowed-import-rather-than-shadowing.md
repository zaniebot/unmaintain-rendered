```yaml
number: 10388
title: Remove shadowed import, rather than shadowing import, in F811
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
base: main
head: charlie/f
created_at: 2024-03-13T15:53:03Z
updated_at: 2024-03-13T16:06:42Z
url: https://github.com/astral-sh/ruff/pull/10388
synced_at: 2026-01-12T15:55:32Z
```

# Remove shadowed import, rather than shadowing import, in F811

---

_@charliermarsh_

## Summary

Today, in F811, if you have code like:

```
import datetime
from datetime import datetime
```

We flag a violation on the second line (the line of the redefinition), and the fix we generate is a removal of the second import. In #10387, I removed the fix in these cases, when the imports map to different symbols.

It seems "more correct", though to remove the first import, since the second import is the one that will _actually_ be used in practice. This PR changes that behavior.

However, I'm sure if this is actually correct, because we're now changing code that's far away from the violation itself. There's an example in our test suite that shows why this isn't ideal:

```python
from typing import (
    Sequence  # noqa
)

from typing import (
    Sequence
)
```

In this case, we'd remove the `Sequence  # noqa` line, since the `# noqa` isn't on the line containing the F811 violation. (I'm not really interested in parsing out the `# noqa` violation from the `Sequence  # noqa` line, because it risks breaking a bunch of assumptions about how `# noqa` works.)

Anyway, interested in feedback.


---

_Comment by @charliermarsh on 2024-03-13 15:58_

I think this isn't a great idea.

---

_Closed by @charliermarsh on 2024-03-13 15:58_

---

_Comment by @github-actions[bot] on 2024-03-13 16:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
