---
number: 11426
title: Implement --allow-global-unused-variables from PyLint
type: issue
state: open
author: Mellowdv
labels:
  - type-inference
assignees: []
created_at: 2024-05-14T14:51:28Z
updated_at: 2024-10-24T14:33:03Z
url: https://github.com/astral-sh/ruff/issues/11426
synced_at: 2026-01-10T01:22:51Z
---

# Implement --allow-global-unused-variables from PyLint

---

_Issue opened by @Mellowdv on 2024-05-14 14:51_

PyLint allows users to pass --allow-global-unused-variables=n(o) option to catch not only function scope unused variables, but also module scope variables. This would modify rule F841: Unused Variable.

I would like to implement this.
Now there is a question that I would like to have answered before committing and perhaps open a discussion on it, if no clear answer is available.

Is 1-to-1 (or let's say bug-for-bug) parity with PyLint desirable?
PyLint has some undesirable behaviour, for example reporting class definitions not used in the same file as the definition as "unused variables" (in our code base and probably most code bases, it is fairly normal to have classes defined in one file and using them in another). I would prefer to define a "global-unused-variable" as not within function scope, not within class scope, not within generator/lambda scope and unused.

As mentioned above, PyLint reports the classes as unused (at least in our code base), and so it seems that it does not check this between files.

As such, I think Ruff is capable of checking whether a Module scoped variable that is not in a Function, Class, Generator or a Lambda, is unused and reporting it (given 'allow-global-unused-variables = false' is set in ruff.toml or other config file). In fact, I've got it working more or less on our codebase, just need to check whether the behaviour is in line with my expectations.

Please let me know if I'm wrong and what the opinion on PyLint parity is.


---

_Label `multifile-analysis` added by @MichaReiser on 2024-05-27 07:52_

---

_Comment by @njharman on 2024-05-27 13:35_

Your implementation is what I would expect. I would be annoyed by the pylint behavior.

Recently an unused global var caused bug for me. Found this issue while searching for "why is this not reported?" So, I'm +1 on this feature being implemented.

---

_Label `type-inference` added by @MichaReiser on 2024-10-24 14:32_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:33_

---
