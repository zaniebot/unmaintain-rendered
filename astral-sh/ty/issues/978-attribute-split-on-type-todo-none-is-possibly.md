```yaml
number: 978
title: "Attribute `split` on type `@Todo | None` is possibly unbound"
type: issue
state: closed
author: MamadouSDiallo
labels:
  - needs-mre
assignees: []
created_at: 2025-08-13T21:47:21Z
updated_at: 2025-08-15T19:06:43Z
url: https://github.com/astral-sh/ty/issues/978
synced_at: 2026-01-12T15:54:24Z
```

# Attribute `split` on type `@Todo | None` is possibly unbound

---

_@MamadouSDiallo_

### Summary

In the code below, I expected that ty would allow `split()` because from the condition, `level_str` is a string and not None. 

```py
if param_est.level is not None and isinstance(param_est.level, str):
            level_str = param_est.level
            varnames = level_str.split("__by__")
```

### Version

_No response_

---

_Label `needs-mre` added by @carljm on 2025-08-13 21:55_

---

_Comment by @carljm on 2025-08-13 21:55_

Thanks for the report! Can you give a more complete code example? My attempt to construct something similar [seems to work as expected](https://play.ty.dev/ccd06ebc-4e3a-4675-bb41-cd9beb1d8b0e), so I'm not sure what is different in your case.

---

_Comment by @MamadouSDiallo on 2025-08-15 16:56_

Here is an example 

```py

from typing import Self

class ParamEst():
    level: str | float | int | None
    est: float | int
    std_err: float | int


class CellEst():
    rowvar: str
    colvar: str
    est: float | int
    std_err: float | int


    @classmethod
    def from_param(cls, param_est: ParamEst) -> Self:
        if param_est.level is not None and isinstance(param_est.level, str):
            level_str = param_est.level
            varnames = level_str.split("__by__")
            rowvar = varnames[0]
            colvar = varnames[1]

        else:
            rowvar = ""
            colvar = ""

        return cls(
            rowvar=rowvar,
            colvar=colvar,
            est=param_est.est,
            std_err=param_est.std_err,
        )
```

---

_Comment by @jelle-openai on 2025-08-15 17:00_

That sample produces no errors in [the playground](https://play.ty.dev/4251e196-e8d4-4c4c-9198-a411ed812ae8).

---

_Comment by @carljm on 2025-08-15 17:01_

@MamadouSDiallo Thanks for the example! That example [seems to type-check correctly in ty](https://play.ty.dev/22cb1fb5-b9fe-493f-8bb6-57b858876ab4), with the desired type narrowing on `param_est.level` and no error on `level_str.split` call. So it seems there's no issue here?

---

_Closed by @carljm on 2025-08-15 17:01_

---

_Comment by @MamadouSDiallo on 2025-08-15 17:07_

Maybe my ty is not up to date or my rules are different from the ones in the playground 

---

_Comment by @carljm on 2025-08-15 17:09_

> Maybe my ty is not up to date or my rules are different from the ones in the playground

None of the rules that would impact this case are suppressed in the playground, so I don't think that can be it.

ty version does seem like a good thing to check.

---

_Comment by @MamadouSDiallo on 2025-08-15 19:06_

I upgraded to the current version, and it runs fine now. Thanks.

---
