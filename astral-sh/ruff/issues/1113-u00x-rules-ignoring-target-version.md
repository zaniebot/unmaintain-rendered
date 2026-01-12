```yaml
number: 1113
title: U00X rules ignoring target-version
type: issue
state: closed
author: bryevdv
labels: []
assignees: []
created_at: 2022-12-06T22:47:43Z
updated_at: 2022-12-07T01:10:14Z
url: https://github.com/astral-sh/ruff/issues/1113
synced_at: 2026-01-12T15:54:41Z
```

# U00X rules ignoring target-version

---

_@bryevdv_

ref: https://github.com/bokeh/bokeh/pull/12653#issuecomment-1331091295

With `target-version = "py38"` we were seeing errors with auto-fix diffs like:

```diff
-TypeOrInst: TypeAlias = Union[Type[T], T]
+TypeOrInst: TypeAlias = type[T] | T
 
-Init: TypeAlias = Union[T, UndefinedType, IntrinsicType]
+Init: TypeAlias = T | UndefinedType | IntrinsicType
```
but `type` is not usable as a replacement for `Type` until Python 3.9 (https://peps.python.org/pep-0585/)

---

_Comment by @charliermarsh on 2022-12-06 23:29_

Very odd, will take a look!

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-06 23:31_

---

_Comment by @charliermarsh on 2022-12-06 23:37_

This was a regression that was introduced in v0.0.145 but fixed in v0.0.151 (https://github.com/charliermarsh/ruff/issues/979).

---

_Comment by @charliermarsh on 2022-12-06 23:38_

@bryevdv - Would you be open to a PR on Bokeh upgrading Ruff to latest? (The `U` prefix was changed to `UP` (backwards-compatible but shows a warning), and `M001` was moved to `RUF100`. Would be happy to upgrade you to insulate against the churn.)


---

_Comment by @charliermarsh on 2022-12-06 23:39_

(Just closing since I believe this is fixed upstream, but do let me know if you want a free upgrade!)

---

_Closed by @charliermarsh on 2022-12-06 23:39_

---

_Comment by @bryevdv on 2022-12-07 00:14_

Thanks for the offer @charliermarsh but we are going to pin and update at a slower intentional pace (probably monthly) 😄  

---

_Comment by @charliermarsh on 2022-12-07 01:10_

Sounds good. I’m always happy to help with version bumps so don’t hesitate to reach out! (The check code changes described above should be the only backwards-incompatible changes thus far.)

---
