```yaml
number: 3053
title: "`A002`: shadowing python builtin bug. "
type: issue
state: closed
author: jonathan-s
labels:
  - question
assignees: []
created_at: 2023-02-20T08:07:47Z
updated_at: 2023-05-22T14:07:15Z
url: https://github.com/astral-sh/ruff/issues/3053
synced_at: 2026-01-12T15:54:43Z
```

# `A002`: shadowing python builtin bug. 

---

_@jonathan-s_

Ruff version: 0.0.247

```
class BaseMessage(dict):
    pass

    @property
    def type(self):
        return self["type"]
```

When using flake8-builtins linting, it complains that `Argument `type` is shadowing a python builtin`. However in this instance it is not an argument, but an attribute. 

This collides with `A003`, right now `A002` takes precedence over `A003`. 

Somewhat interesting discussion around `A003` -> https://github.com/gforcada/flake8-builtins/issues/75 though I would say, leave it up to the user. Personally I don't see much point in A003. 

---

_Renamed from "flake8-builtin: shadowing python builtin bug. " to "A002: flake8-builtin: shadowing python builtin bug. " by @jonathan-s on 2023-02-20 08:08_

---

_Renamed from "A002: flake8-builtin: shadowing python builtin bug. " to "`A002`: shadowing python builtin bug. " by @jonathan-s on 2023-02-20 08:08_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-20 23:25_

---

_Label `bug` added by @charliermarsh on 2023-02-20 23:25_

---

_Unassigned @charliermarsh by @charliermarsh on 2023-02-20 23:25_

---

_Comment by @charliermarsh on 2023-02-21 22:21_

When I run this, I get `A003 Class attribute `type` is shadowing a python builtin`, which seems correct. Am I misinterpreting the issue?

---

_Label `bug` removed by @charliermarsh on 2023-02-21 22:21_

---

_Label `question` added by @charliermarsh on 2023-02-21 22:21_

---

_Closed by @charliermarsh on 2023-02-24 23:01_

---

_Comment by @allisonkarlitskaya on 2023-05-22 14:07_

I think the issue is that it doesn't really make sense to worry about this problem because it's almost never possible to refer to class attributes in a way that would conflict with a builtin.

An interesting approach might be to allow the definition to go through but to flag any potentially-confusing use.

It's also easy enough to just explicitly exclude `A003` (or don't select `A` in the first place).  It might make sense to remove `A003` from being automatically enabled by `A`, though.

---
