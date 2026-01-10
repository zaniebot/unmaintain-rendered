```yaml
number: 275
title: Implement A001, A002 (flake8-builtins)
type: issue
state: closed
author: charliermarsh
labels:
  - good first issue
  - rule
assignees: []
created_at: 2022-09-28T21:36:26Z
updated_at: 2022-09-29T20:46:16Z
url: https://github.com/astral-sh/ruff/issues/275
synced_at: 2026-01-10T15:56:05Z
```

# Implement A001, A002 (flake8-builtins)

---

_Issue opened by @charliermarsh on 2022-09-28 21:36_

See: https://github.com/gforcada/flake8-builtins


---

_Label `enhancement` added by @charliermarsh on 2022-09-28 21:36_

---

_Label `good first issue` added by @charliermarsh on 2022-09-28 21:37_

---

_Label `enhancement` removed by @charliermarsh on 2022-09-28 21:38_

---

_Label `rule` added by @charliermarsh on 2022-09-28 21:38_

---

_Comment by @sobolevn on 2022-09-28 21:43_

Hi, I would like to work on it :)

---

_Comment by @charliermarsh on 2022-09-28 23:33_

Awesome! Feel free to punt on the autofixing if it gets tricky. The autofix API still relies on manual token parsing.

---

_Comment by @sobolevn on 2022-09-29 06:50_

I am very reluctant about adding autofixing to naming rules. Naming should be done by humans.
`type` -> `type_` is just not good enough.

Moreover, autofixing naming can actually break stuff. Imagine that some `copyright` or `type` name is actually required by a third-party API.


---

_Comment by @adriangb on 2022-09-29 07:04_

For the little my opinion is worth I regularly do stuff like `id = ...` or `type = ...` because I think `id_ = ...` or `tp = ...` convolutes the name and my type checker will yell at me already if I try to do `id = 1; id(...)`. This is especially true in small self contained functions (that's what namespaces are for). The auto fix would break my code in strange ways. And it's very possible the person running the auto fix is not me.

---

_Comment by @charliermarsh on 2022-09-29 07:18_

Sorry, yes, this rule absolutely should not have autofix. (I wrote that comment from my phone and had #273 in my head.)

---

_Comment by @charliermarsh on 2022-09-29 20:38_

Resolved by #284!

---

_Closed by @charliermarsh on 2022-09-29 20:38_

---

_Comment by @sobolevn on 2022-09-29 20:43_

Thanks! I will work on something else tomorrow :)

---

_Comment by @charliermarsh on 2022-09-29 20:46_

Thank _you_!

P.S. Now that you've gone through the end-to-end process of implementing a rule, feel free to propose any broader refactors that might make that process easier / saner.


---
