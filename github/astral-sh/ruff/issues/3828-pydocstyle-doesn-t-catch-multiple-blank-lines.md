---
number: 3828
title: "pydocstyle doesn't catch multiple blank lines between sections"
type: issue
state: open
author: sanzoghenzo
labels:
  - docstring
assignees: []
created_at: 2023-03-31T10:19:04Z
updated_at: 2023-05-25T00:43:50Z
url: https://github.com/astral-sh/ruff/issues/3828
synced_at: 2026-01-07T13:12:14-06:00
---

# pydocstyle doesn't catch multiple blank lines between sections

---

_Issue opened by @sanzoghenzo on 2023-03-31 10:19_

Hi there, thank you for this awesome tool!!!

I just incurred in this issue: if I have multiple blank lines between google style docstring sections, no errors and no fixes are run.

```python
def test(variable: str) -> str:
    """
    Perfoms a basic operation on variable.

    Args:
        variable: input string.





    References:
        test

    Returns:
        Validated dataframe of the Excel data.
    """
   return variable
```

I suppose this is  the exact interpretation of D410 and D411 (missing blank lines before/after section), but it seems to me that there should be only one line between them.

I see that there is #3827 already, but it seems a bit different (and I could not reproduce that issue with google style docstrings).

---

_Label `docstring` added by @charliermarsh on 2023-03-31 14:27_

---

_Comment by @JBLDKY on 2023-04-01 06:00_

Thank you for filing the issue! 

I will be looking into this today. 

---

_Comment by @JBLDKY on 2023-04-01 08:24_

I checked how pydocstyle handles this situation and I couldn't find anything. I like the idea of having the sections be separated by strictly 1 line. However, my interpretation of D410 and D411 do not necessarily encompass this behaviour. As such I'm curious what the team thinks about this.

Perhaps moving it to a new rule would make more sense?

If it hypothetically were moved to a new rule / separate check / separate fix it would make it easier to catch both this issue and #3827 in one.

---

_Comment by @sanzoghenzo on 2023-04-01 08:40_

Yeah, I forgot to mention that also pydocstyle doesn't catch this issue, so you're right it should be a new rule to keep the rule behaviour consistent. 

This could be named "exactly-one-line-between-sections"

---

_Renamed from "pycodestyle doesn't catch multiple blank lines between sections" to "pydocstyle doesn't catch multiple blank lines between sections" by @charliermarsh on 2023-05-25 00:43_

---
