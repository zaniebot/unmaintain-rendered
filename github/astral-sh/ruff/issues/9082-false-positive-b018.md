---
number: 9082
title: "False positive: B018"
type: issue
state: closed
author: wmortal
labels: []
assignees: []
created_at: 2023-12-10T23:34:42Z
updated_at: 2023-12-11T15:32:34Z
url: https://github.com/astral-sh/ruff/issues/9082
synced_at: 2026-01-07T13:12:15-06:00
---

# False positive: B018

---

_Issue opened by @wmortal on 2023-12-10 23:34_

Ruff 0.1.7
pyproject.toml:
```
[tool.ruff.lint]
extend-select = ["B"]  # flake8-bugbear
```
Command:
`ruff check /tmp/ruff/example.py`

Source:
```
class Example:
    @property
    def name(self) -> str:
        return "example"


example = Example()
example.name
^^^^^^^^^^^^
B018 Found useless expression. Either assign it to a variable or remove it

with pytest.raises(ValueError):
    example.name
    ^^^^^^^^^^^^
    B018 Found useless expression. Either assign it to a variable or remove it

```

I never encountered that error in a production code, but I have a lot of it in my tests, that check if calling that property raises an error (second example of the error). Similarly to what was described in #3831. I understand that this is context-specific and in some situations it would be better to get that error even for the cost of a few false-positives. Or maybe it could be less strict in a context of test files? Removing the _property_ decorator and calling it as a method fixes the error.

---

_Renamed from "False positrive: B018" to "False positive: B018" by @wmortal on 2023-12-10 23:38_

---

_Comment by @dhruvmanila on 2023-12-11 15:32_

Hey, thanks for providing a detailed issue. Do you think the solution mentioned in the linked issue (https://github.com/astral-sh/ruff/issues/3831#issuecomment-1493202616) can help your use-case?

> Removing the property decorator and calling it as a method fixes the error.

Yes, this observation is correct. We avoid triggering this rule if the expression can potentially contain any side effects like a function call.

I'll close this in favor of the linked issue.

---

_Closed by @dhruvmanila on 2023-12-11 15:32_

---
