```yaml
number: 5177
title: "Consider either reporting or accepting #noqa (without space)"
type: issue
state: closed
author: mairas
labels:
  - bug
  - suppression
assignees: []
created_at: 2023-06-19T09:27:59Z
updated_at: 2023-07-06T16:03:12Z
url: https://github.com/astral-sh/ruff/issues/5177
synced_at: 2026-01-10T11:09:47Z
```

# Consider either reporting or accepting #noqa (without space)

---

_Issue opened by @mairas on 2023-06-19 09:27_

I just tried to add a `noqa` comment on my Pydantic validator that doesn't get correctly introspected:

```
import pydantic


class Device(pydantic.BaseModel):
    serial: int

    @pydantic.validator("serial")
    def validate_serial(cls, v: int) -> int:  #noqa: N805
        return v
```

But as a typo, didn't add a space between the comment # mark and the `noqa` pragma. When running `ruff check --fix`, the `noqa` got ignored and ruff reported an error. `black` would fix these errors but it is run after `ruff` (due to `--fix`). Needless to say, this resulted in quite a bit of confused debugging. :-)

My first idea would be to implement a new rule that would report such errors. Or alternatively, `ruff` could accept `noqa` pragmas even without the preceding whitespace.

Ruff settings:

```
[tool.ruff] # https://github.com/charliermarsh/ruff
fix = true
ignore = [
  "B904",
  "B905",
  "BLE001",
  "E501",
  "ERA001",
  "N802",
  "N803",
  "PGH003",
  "PLR2004",
  "RET504",
  "S101",
  "S104",
]
ignore-init-module-imports = true
line-length = 88
select = [
  #"A",
  "B",
  "BLE",
  #"C4",
  #"C90",
  #"D",
  #"DTZ",
  "E",
  "ERA",
  "F",
  "G",
  "I",
  "INP",
  "ISC",
  "N",
  "NPY",
  "PGH",
  "PIE",
  "PLC",
  "PLE",
  "PLR",
  #"PLW",
  "PT",
  "PTH",
  "PYI",
  #"RET",
  "RSE",
  "RUF",
  "S",
  #"SIM",
  "T10",
  "T20",
  "TID",
  "UP",
  "W",
  "YTT",
]
target-version = "py310"
unfixable = ["ERA001", "F841", "T201", "T203"]
```

Ruff version: 0.0.265


---

_Label `question` added by @charliermarsh on 2023-06-20 20:54_

---

_Label `rule` added by @charliermarsh on 2023-06-20 20:54_

---

_Label `rule` removed by @charliermarsh on 2023-06-20 20:54_

---

_Label `noqa` added by @charliermarsh on 2023-06-20 20:54_

---

_Label `question` removed by @charliermarsh on 2023-07-04 19:40_

---

_Label `bug` added by @charliermarsh on 2023-07-04 19:40_

---

_Closed by @charliermarsh on 2023-07-06 16:03_

---
