```yaml
number: 14135
title: "A006 and A002 both trigger for lambda's"
type: issue
state: closed
author: jaap3
labels:
  - bug
assignees: []
created_at: 2024-11-06T14:51:06Z
updated_at: 2024-11-07T05:34:10Z
url: https://github.com/astral-sh/ruff/issues/14135
synced_at: 2026-01-10T11:09:55Z
```

# A006 and A002 both trigger for lambda's

---

_Issue opened by @jaap3 on 2024-11-06 14:51_

This example triggers two violations (with preview enabled), is this intentional?

```python
lambda id: id + 1
```

```
$ ruff check --isolated --preview --select A test.py

test.py:1:8: A002 Argument `id` is shadowing a Python builtin
  |
1 | lambda id: id + 1
  |        ^^ A002
  |

test.py:1:8: A006 Lambda argument `id` is shadowing a Python builtin
  |
1 | lambda id: id + 1
  |        ^^ A006
  |
```

```
$ ruff --version
ruff 0.7.2
```

---

_Label `bug` added by @dylwil3 on 2024-11-06 15:27_

---

_Label `needs-decision` added by @dylwil3 on 2024-11-06 15:29_

---

_Comment by @dylwil3 on 2024-11-06 15:30_

Nice catch! It looks like `flake8-builtins` distinguishes between (named) functions and lambdas. We should match that behavior and maybe update the documentation on `A002` and `A006`.

My only hesitation is that this would be a breaking change for a stable rule - so if people were running the checker with preview _off_ and catching this shadowing on lambdas, they would no longer catch it if we matched `flake8-builtins`. 

Not sure the best protocol for handling this - @MichaReiser / @AlexWaygood ?

---

_Comment by @AlexWaygood on 2024-11-06 18:06_

Great question. When we _expand_ the scope of a stable rule, we generally put the new behaviour in preview mode for a couple of months before promoting it to stable. When we _reduce_ the scope of a rule, it's more of a judgement call.

I _can_ see the argument that this might be a breaking change for some people -- but it definitely feels like a bug to me to have two rules complain about the exact same error. So I'd be inclined to ship the behaviour change straight into stable for this one, so that users can benefit from the bugfix even if they haven't selected `preview = true` (most don't).

---

_Label `needs-decision` removed by @dylwil3 on 2024-11-06 18:28_

---

_Renamed from "A005 and A002 both trigger for lambda's" to "A006 and A002 both trigger for lambda's" by @dylwil3 on 2024-11-07 00:54_

---

_Assigned to @dylwil3 by @dylwil3 on 2024-11-07 01:30_

---

_Closed by @dylwil3 on 2024-11-07 05:34_

---
