---
number: 19274
title: "ban import style (`from module import thing` vs `import module`) (banned api)"
type: issue
state: closed
author: fraser-langton
labels:
  - question
assignees: []
created_at: 2025-07-11T05:07:56Z
updated_at: 2025-07-11T16:07:48Z
url: https://github.com/astral-sh/ruff/issues/19274
synced_at: 2026-01-07T13:12:16-06:00
---

# ban import style (`from module import thing` vs `import module`) (banned api)

---

_Issue opened by @fraser-langton on 2025-07-11 05:07_

### Summary

Similar to [banned-api (TID251)](https://docs.astral.sh/ruff/rules/banned-api/#banned-api-tid251)

Would be nice sometimes to enforce how a module is imported - when I have clashing namespaces I like to do this by convention in bigger projects would be nice to enforce it.


```python
from models import User
from schema import User as UserSchema

user = User()
user_response = UserSchema.model_validate(user)
```


```python
import models
import schema

user = models.User()
user_response = schema.User.model_validate(user)
```

maybe you want to ban one of these import styles

---

_Renamed from "Banned API - ban import style (`from module import thing` vs `import module`)" to "ban import style (`from module import thing` vs `import module`) (banned api)" by @fraser-langton on 2025-07-11 05:08_

---

_Comment by @MichaReiser on 2025-07-11 08:30_

Would [`banned-from`](https://docs.astral.sh/ruff/settings/#lint_flake8-import-conventions_banned-from) work for you? 

---

_Label `question` added by @MichaReiser on 2025-07-11 08:30_

---

_Comment by @fraser-langton on 2025-07-11 08:34_

I think that would work, at least in the use cases I have I want to ban from imports - it's probably less likely that someone would want to enforce from imports right?

---

_Comment by @fraser-langton on 2025-07-11 08:38_

...just realised you linked something that already exists 🤦‍♂️

I will give this a try, my only concern is how the behaviour works for your own packages, ie you would be more likely to import from within the module but would enforce banned-from elsewhere in the code

---

_Comment by @fraser-langton on 2025-07-11 08:59_

after giving it a go I think it should serve my use case, whilst it being a ruff rule instead of top-level would allow to ban the pattern more specifically - [banned-from](https://docs.astral.sh/ruff/settings/#lint_flake8-import-conventions_banned-from) works for me

Not sure how I didn't find it, I both did a bit of googling and asked LLMs

---

_Comment by @MichaReiser on 2025-07-11 16:07_

No worries. I'll close this then.

---

_Closed by @MichaReiser on 2025-07-11 16:07_

---
