```yaml
number: 978
title: PyCharm Inspections support
type: issue
state: closed
author: nikolaik
labels: []
assignees: []
created_at: 2022-12-01T07:59:19Z
updated_at: 2022-12-02T18:25:46Z
url: https://github.com/astral-sh/ruff/issues/978
synced_at: 2026-01-10T12:09:58Z
```

# PyCharm Inspections support

---

_Issue opened by @nikolaik on 2022-12-01 07:59_

Hi there,

More of a question than a request, have you looked into supporting any of the [PyCharm inspections](https://www.jetbrains.com/help/pycharm/code-inspection.html)? I found a lot of overlap with the existing ruff and to be implemented pylint rules though, but there might be some worth checking out.

Does ruff support something similar to the  _Redeclared names without usage_ one (like I requested [here](https://github.com/PyCQA/pylint/issues/3471))?

PS: Seeing all the good progress being made with ruff is super exciting 💖 

---

_Comment by @charliermarsh on 2022-12-01 15:26_

Thank you!

I haven't looked closely but I mostly use PyCharm so I _am_ familiar with them. I do want to add unused argument detection soon.

Would _Redeclared names without usage_ be equivalent to Flake8's [F811](https://www.flake8rules.com/rules/F811.html)?


---

_Comment by @nikolaik on 2022-12-01 17:04_

Nice!

> Would _ Redeclared names without usage_ be equivalent to Flake8's [F811](https://www.flake8rules.com/rules/F811.html)?

That is very similar at least, by the description it looks like it detects redeclared imports only. I believe the pycharm rule works on any attribute. An example would be:

```py
class A:
    b = "ninja"
    b = "turtles"
```
Or like the old example in the linked pylint issue:

```py
class OptionType(graphene.InputObjectType):
    freeform_enabled = graphene.Boolean(required=True)
    freeform_enabled = GenericScalar(required=True)
``` 

---

_Comment by @charliermarsh on 2022-12-02 18:25_

Gonna close this for now as F811 is being tracked separately (and I'd like it to be as expansive as is suggested here).

---

_Closed by @charliermarsh on 2022-12-02 18:25_

---
