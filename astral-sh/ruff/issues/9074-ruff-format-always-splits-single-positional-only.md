---
number: 9074
title: Ruff format always splits single positional-only argument list across two lines
type: issue
state: closed
author: drhagen
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-12-09T18:49:09Z
updated_at: 2023-12-09T23:03:32Z
url: https://github.com/astral-sh/ruff/issues/9074
synced_at: 2026-01-10T01:22:48Z
---

# Ruff format always splits single positional-only argument list across two lines

---

_Issue opened by @drhagen on 2023-12-09 18:49_

Black [keeps this code as is](https://black.vercel.app/?version=stable&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4ADVAHpdAD2IimZxl1N_WlbvK5V_r8p8RWVQhH4T2O7WqUDeXrzfa-e6wziIBZ7QgnOLLS6AI8_u-ndkugsJo2YBSDMHN0D47vXymeQ1jtqapcY5xyV7JpggMWoOxoxwKgPXPN0A2MQwkZu3vmIU6QGNqrpOmboAPkPAchIEDU0AAAAAXBH-g4MbwnEAAZYB1gEAAEGE9zexxGf7AgAAAAAEWVo=):

```python
def some_function(
    string: str, /
) -> ReallyReallyReallyReallyReallyReallyReallyReallyLongName:
    pass
```

But Ruff 0.1.7 forces the argument list to be split across lines.

```python
def some_function(
    string: str,
    /,
) -> ReallyReallyReallyReallyReallyReallyReallyReallyLongName:
    pass
```

I would also consider Black's behavior to be more desirable here. This is a regression from Ruff 0.1.6. I suspect that this was caused by #8921, where it would be easy to think this function has one argument. Semantically, it does, while syntactically, it has two.

---

_Comment by @charliermarsh on 2023-12-09 19:19_

Interesting, that makes sense... Thanks for reporting!

---

_Label `bug` added by @charliermarsh on 2023-12-09 19:19_

---

_Label `formatter` added by @charliermarsh on 2023-12-09 19:19_

---

_Comment by @charliermarsh on 2023-12-09 19:20_

Should be straightforward to fix. I appreciate you finding + linking back to the relevant PR.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-09 19:25_

---

_Referenced in [astral-sh/ruff#9076](../../astral-sh/ruff/pulls/9076.md) on 2023-12-09 19:57_

---

_Closed by @charliermarsh on 2023-12-09 23:03_

---
