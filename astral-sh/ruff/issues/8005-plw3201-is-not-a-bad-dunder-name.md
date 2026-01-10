```yaml
number: 8005
title: "PLW3201 \"_\" is not a bad dunder name"
type: issue
state: closed
author: NeilGirdhar
labels:
  - bug
assignees: []
created_at: 2023-10-17T09:45:35Z
updated_at: 2023-10-17T14:13:06Z
url: https://github.com/astral-sh/ruff/issues/8005
synced_at: 2026-01-10T11:09:50Z
```

# PLW3201 "_" is not a bad dunder name

---

_Issue opened by @NeilGirdhar on 2023-10-17 09:45_

`_` is not a bad dunder name.  `_` is often used functions whose names are not relevant.  Please consider excluding `_` from PLW3201's rules.:

```python
    prediction_energy = custom_jvp_method(  # type: ignore[assignment] # pyright: ignore
        prediction_energy, nondiff_argnums=(0, 2, 3))

    @prediction_energy.defjvp  # type: ignore[no-redef, attr-defined, misc]
    def _(self,
          observation: FisherMessage,
          weights: Weights,
          primals: tuple[RivalMessage],
          tangents: tuple[RivalMessage]
          ) -> tuple[JaxRealArray, JaxRealArray]:
        natural_explanation, = primals
```

---

_Comment by @NeilGirdhar on 2023-10-17 09:49_

Incidentally, not sure where to write this, but the previewed rules that were useful to me were: 

* E114 Indentation is not a multiple of 4 (comment)
* E116 Unexpected indentation (comment)
* E203 [*] Whitespace before ','
* E241 Multiple spaces after comma
* E271 Multiple spaces after keyword

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-17 13:50_

---

_Comment by @charliermarsh on 2023-10-17 13:54_

Seems reasonable, it's almost certainly intentional.

---

_Comment by @zanieb on 2023-10-17 13:55_

@NeilGirdhar perhaps you're looking for https://github.com/astral-sh/ruff/issues/7993?

---

_Label `bug` added by @charliermarsh on 2023-10-17 13:57_

---

_Closed by @charliermarsh on 2023-10-17 14:13_

---
