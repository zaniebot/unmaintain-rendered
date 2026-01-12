```yaml
number: 767
title: Imported but not used fix under conditional as last import
type: issue
state: closed
author: LefterisJP
labels:
  - bug
assignees: []
created_at: 2022-11-16T13:16:27Z
updated_at: 2022-11-16T15:55:46Z
url: https://github.com/astral-sh/ruff/issues/767
synced_at: 2026-01-12T15:54:40Z
```

# Imported but not used fix under conditional as last import

---

_@LefterisJP_

Hey I got a kind of an edge case to report but guess it may be fixable so thought why not make an issue.

Consider the following

```python
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from random import randint
    from typing import List

a = 1
```

If you run `ruff --fix` on this it will detect that the 2 imports unde the if are not used and will delete them. But this will leave the `if TYPE_CHECKING` block empty, which is a syntax error.

Would it be possible to detect this and also delete the if?

Funnily enough if I only put one unused import, ruff fix will add a `pass` under the if which will fix the syntax error.

Another corollary of this is that a second run of ruff should also delete the `from typing import TYPE_CHECKING` since it's also unused. Could those two runs be combined?

---

_Comment by @charliermarsh on 2022-11-16 13:31_

Thanks for filing! The intended behavior is to remove the two imports in the if-block, and replace them with a pass… so sounds like a bug if it’s leaving you with an empty body.

I’d been hesitant to remove the if itself since it introduces a lot of complexity, but in theory it’s possible…

---

_Label `bug` added by @charliermarsh on 2022-11-16 13:31_

---

_Comment by @charliermarsh on 2022-11-16 13:51_

Can I just confirm: are you on the latest version? I’m seeing Ruff correctly add a pass statement in that example.

---

_Comment by @LefterisJP on 2022-11-16 15:17_

Hey @charliermarsh apologies. I should have checked. I was in an 1 week older venv and had `ruff 0.0.105` there. With `ruff 0.0.122` I also see the correct behavior.

---

_Closed by @LefterisJP on 2022-11-16 15:17_

---

_Comment by @charliermarsh on 2022-11-16 15:55_

No prob! That was a regression anyway. Glad it’s working now.

---
