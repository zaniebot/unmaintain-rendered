```yaml
number: 4209
title: Support for pycodestyle publicity
type: issue
state: closed
author: henryborchers
labels:
  - docstring
assignees: []
created_at: 2023-05-03T14:54:21Z
updated_at: 2023-05-04T14:53:20Z
url: https://github.com/astral-sh/ruff/issues/4209
synced_at: 2026-01-12T15:54:44Z
```

# Support for pycodestyle publicity

---

_@henryborchers_

Maybe I'm missing something. In pydocstyle, D1xx checks are only for items that are considered to be public. If  __all__ is set at in the module, only those classes, function, etc are checked.  (see https://www.pydocstyle.org/en/2.1.1/error_codes.html#publicity)

When I tried running for the same checks in Ruff, I get the rule violations everything (for example: "D102 Missing docstring in public method")  in that module, whether it's listed in __all__ or not. 

My config in my pyproject.toml file. 

```
...
[tool.ruff]
select = ["E", "F", "D"]

[tool.ruff.pydocstyle]
convention = "google"
```

Am I missing something?

---

_Comment by @henryborchers on 2023-05-03 14:58_

An example of what I mean.
```

__all__ = ['MyPublicClass']


class MyPublicClass:
    def __init__(self):
        # This one should have a D107
        pass

class MyPrivateClass:
    def __init__(self):
        # This one should NOT have a D107
        pass

```

---

_Comment by @charliermarsh on 2023-05-04 02:46_

Agreed, we should support this. I think this is the same as #993, so closing just to deduplicate, but let me know if I'm mistaken.

---

_Closed by @charliermarsh on 2023-05-04 02:46_

---

_Label `docstring` added by @charliermarsh on 2023-05-04 02:47_

---

_Comment by @henryborchers on 2023-05-04 14:52_

D'oh!! I was looking for an existing one to avoid duplicates but missed it. That's 99% of what I'm asking about. Sorry about creating the dup. I'll comment on the other issue.

---

_Comment by @charliermarsh on 2023-05-04 14:53_

No need to apologize at all. For better or worse I tend to have most of the Ruff issues sitting somewhere in my brain. It's a lot easier for a me to remember a duplicate than for a contributor to comb through all the issues matching "docstring" :joy:

---
