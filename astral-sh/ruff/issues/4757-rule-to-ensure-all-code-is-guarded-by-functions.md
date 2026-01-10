```yaml
number: 4757
title: Rule to ensure all code is guarded by functions or __main__
type: issue
state: open
author: quassy
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-05-31T14:41:04Z
updated_at: 2025-08-23T15:44:10Z
url: https://github.com/astral-sh/ruff/issues/4757
synced_at: 2026-01-10T11:09:47Z
```

# Rule to ensure all code is guarded by functions or __main__

---

_Issue opened by @quassy on 2023-05-31 14:41_

I believe there is a flake8 plugn for this but I can't find it, also not a rule for ruff: Quite often I have to make sure that to avoid unintended side effects of imports all regular code should be guarded by functions, classes or `if __name__ == "__main__":`

```py
from sqlalchemy import create_engine

engine = create_engine("postgresql+psycopg2://scott:tiger@localhost:5432/mydatabase")
```

The inspection could look like this:
```
script.py:3:1: FOO999 Code is not guarded by function or if __name__ == "__main__", importing it may have unindented side effects
```

---

I's not safe to autofix because it will certainly change behaviour if you rely on the code being run on import. Manual fixes would be:
```py
from sqlalchemy import create_engine

if __name__ == "__main__":
# or
def get_my_engine():
    engine = create_engine("postgresql+psycopg2://scott:tiger@localhost:5432/mydatabase")
```

---

_Label `rule` added by @charliermarsh on 2023-05-31 15:23_

---

_Comment by @evanrittenhouse on 2023-05-31 17:35_

@charliermarsh feel free to assign this to me, I can take a look. I'll put it under the `RUF` linter. Want to get back into the AST, seems like we'll have to keep track of the scope block and ensure that all statements are associated with a scope?

---

_Comment by @charliermarsh on 2023-05-31 18:13_

It's probably too restrictive of a lint to include in `RUF`, another case where we could use some kind of opt-in pedantic category.

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:27_

---

_Comment by @Avasam on 2025-08-23 15:44_

Even if not used as a rule directly, this kind of analysis would be useful to detect modules causing side effects (https://github.com/astral-sh/ruff/issues/8149)

---
