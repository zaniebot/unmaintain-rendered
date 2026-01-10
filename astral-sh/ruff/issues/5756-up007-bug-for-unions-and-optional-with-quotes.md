```yaml
number: 5756
title: UP007 - Bug for Unions and Optional with Quotes Inside Brackets
type: issue
state: closed
author: max-muoto
labels: []
assignees: []
created_at: 2023-07-14T03:38:48Z
updated_at: 2023-07-15T19:00:17Z
url: https://github.com/astral-sh/ruff/issues/5756
synced_at: 2026-01-10T11:09:48Z
```

# UP007 - Bug for Unions and Optional with Quotes Inside Brackets

---

_Issue opened by @max-muoto on 2023-07-14 03:38_

## Bug Description

Ruff doesn't detect instances of `Union` where there is at-least one quoted type among the unioned types for [UP007(https://beta.ruff.rs/docs/rules/non-pep604-annotation/).

The same goes for quoted types inside of `Optional`.

Take several examples:

- `"Optional[str]"` autofixed -> `"str | None"`
- `"Union[str, int]"` autofixed -> `"str | int"`
- `Union["str", int]` not detected by `ruff check`
- `Union["str", "int"]` not detected by `ruff check`
- `Optional["str"]` not detected by `ruff check`

Obviously once difference here is that `Union["str", int]` is valid, but `"str" | int` will cause a runtime error, so if there was an autofix I'd assume it would just get transformed to `"str | int"`.

## Other Details:
- Command: `ruff check`
- Ruff Settings: `UP007` enabled individually (not the entire `UP` suite).
- Version: `ruff 0.0.278`








---

_Renamed from "UP007 Bug when for Unions with Quotes" to "UP007 Bug for Unions with Quotes" by @max-muoto on 2023-07-14 03:38_

---

_Renamed from "UP007 Bug for Unions with Quotes" to "UP007 - Bug for Unions with Quotes" by @max-muoto on 2023-07-14 03:39_

---

_Renamed from "UP007 - Bug for Unions with Quotes" to "UP007 - Bug for Unions and Optional with Quotes" by @max-muoto on 2023-07-14 03:41_

---

_Renamed from "UP007 - Bug for Unions and Optional with Quotes" to "UP007 - Bug for Unions and Optional with Quotes Inside Brackets" by @max-muoto on 2023-07-14 03:43_

---

_Comment by @charliermarsh on 2023-07-14 04:50_

We now support this in limited cases #5725, when `"str" | int` won't error at runtime (e.g., it's an assignment annotation within a function body). We still don't fix `Union["str", int]` to `"str | int"`, but I'm not totally sure that we should.

---

_Closed by @charliermarsh on 2023-07-14 04:50_

---

_Comment by @charliermarsh on 2023-07-14 04:51_

We could arguably flag-but-not-fix these -- the original behavior was motivated by pyupgrade's decisions (pyupgrade doesn't fix any cases that include a string in the subscript).

---

_Comment by @max-muoto on 2023-07-15 19:00_

> 

I see, that makes sense then, thanks for the context! I do think a flag would at least be helpful, but understand if the goal is just simple parity with pyupgrade.

---
