```yaml
number: 1852
title: E712 (Comparison to True should be cond is True) should be ignored when comparison is against pandas.Series
type: issue
state: closed
author: rsokolewicz
labels:
  - bug
assignees: []
created_at: 2023-01-13T15:55:17Z
updated_at: 2025-04-03T14:36:08Z
url: https://github.com/astral-sh/ruff/issues/1852
synced_at: 2026-01-10T11:09:44Z
```

# E712 (Comparison to True should be cond is True) should be ignored when comparison is against pandas.Series

---

_Issue opened by @rsokolewicz on 2023-01-13 15:55_

using `ruff==0.0.215`

The following snippet raises an E712 when running `ruff`:

``` python
import pandas as pd

df = pd.DataFrame({"x" : [False, True]})
df["x"] != False
```

and the autofix replaces `df["x"] != False` with `df["x"] is not False`. The two are unfortunately not equivalent and the latter leads to errors from pandas. 


---

_Label `bug` added by @charliermarsh on 2023-01-14 03:18_

---

_Comment by @charliermarsh on 2023-01-14 03:18_

Pretty hard to avoid until we can infer that `df` is a DataFrame here.

---

_Comment by @akanz1 on 2023-01-14 09:07_

Can we prevent the autofix from applying this specific rule?

---

_Comment by @nils-borrmann-y42 on 2023-01-14 11:27_

The same problem happens in sqlalchemy queries (`.where(foo != bar)` is not equivalent to `.where(foo is not bar)`)

---

_Comment by @charliermarsh on 2023-01-14 15:12_

Before I disable autofix for this rule entirely... you can disable it for your project via:

```toml
[tool.ruff]
unfixable = ["E712"]
```

Is that sufficient here?


---

_Comment by @akanz1 on 2023-01-14 18:06_

Works for me, thanks! :)

---

_Comment by @rsokolewicz on 2023-01-14 19:36_

In my data analysis and data science projects I end up ignoring this rule more often than having it autofixed, so I ended up using

```
[tool.ruff]
ignore = ["E712"]
```

instead. There are a few other cases where autofixing `== True` to `is True` will break code, namely when comparing against booleans that have a package-specific implementation such as numpy (`numpy.bool_`) or pytorch (`toch.bool`). 

---

_Comment by @charliermarsh on 2023-01-14 22:32_

Yeah, that makes sense. I think I'm going to close this for now given that we have some workarounds (users should either disable the rule entirely, or, if they want the rule to be enabled but want to avoid false fixes, should use `unfixable`).

---

_Closed by @charliermarsh on 2023-01-14 22:32_

---

_Comment by @Faolain on 2023-07-17 19:02_

This just broke our code using SQLAlchemy converting

```
signup_code = (
    db.session.query(SignupCode)
    .filter(SignupCode.code == code, SignupCode.used == False)
    .first()
)
```

to 

```
 signup_code = (
        db.session.query(SignupCode)
        .filter(SignupCode.code == code, SignupCode.used is False)
        .first()
    )
```

which it definitely shouldn't have autofixed. Can this instead be a suggestion provided but something that is not autofixed? Can prevent a lot of headaches. 

---

_Comment by @charliermarsh on 2023-07-17 19:06_

This is marked in the codebase as a "Suggested" autofix, which in a future release will require running with additional command-line flags to explicitly opt-in to fixing.

---

_Comment by @linde-frolke on 2025-03-27 12:57_

For pandas it can be solved by writing 
` ~boolean_series ` instead of `boolean_series == False`.

---

_Comment by @LarsenCundric on 2025-04-03 14:36_

Got the same issue where a sql query `param == False` was "fixed to `not param` which broke the code. 

---
