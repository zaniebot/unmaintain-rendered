```yaml
number: 7355
title: "Formatter undocumented deviation: splits long conditionals into multiple lines"
type: issue
state: closed
author: tjkuson
labels: []
assignees: []
created_at: 2023-09-13T16:48:48Z
updated_at: 2023-09-13T17:15:25Z
url: https://github.com/astral-sh/ruff/issues/7355
synced_at: 2026-01-12T15:54:47Z
```

# Formatter undocumented deviation: splits long conditionals into multiple lines

---

_@tjkuson_

Black:

```python
class Foo:
    def bar():
        assert obj == [
            "kdfsjhslkdfjklsdjflksdjfklsdjfksdsjakldjalksdjlasdjdfjasdkasdfkldshjfkjdsh",
            "kdfsjhslkdfjklsdjflksdjfklsdjfksdsjakldjalksdjlasdjdfjasdkasdfkldshjfkjdsh",
            "kdfsjhslkdfjklsdjflksdjfklsdjfksdsjakldjalksdjlasdjdfjasdkasdfkldshjfkjdsh",
        ]

```

Ruff:

```python
class Foo:
    def bar():
        assert (
            obj
            == [
                "kdfsjhslkdfjklsdjflksdjfklsdjfksdsjakldjalksdjlasdjdfjasdkasdfkldshjfkjdsh",
                "kdfsjhslkdfjklsdjflksdjfklsdjfksdsjakldjalksdjlasdjdfjasdkasdfkldshjfkjdsh",
                "kdfsjhslkdfjklsdjflksdjfklsdjfksdsjakldjalksdjlasdjdfjasdkasdfkldshjfkjdsh",
            ]
        )

```

Ruff 0.0.289 with a line-length of 88.

---

_Comment by @tjkuson on 2023-09-13 16:55_

FWIW, I prefer the Black formatting; it produces shorter lines while being less verbose.

---

_Renamed from "Formatter undocumented deviation: splits long conditionals into multiple lines where Black does not" to "Formatter undocumented deviation: splits long conditionals into multiple lines" by @tjkuson on 2023-09-13 16:55_

---

_Comment by @charliermarsh on 2023-09-13 17:11_

This may be the same as https://github.com/astral-sh/ruff/issues/7246.

---

_Comment by @tjkuson on 2023-09-13 17:13_

It does seem to be, I'll close the issue. Apologies for the noise!

---

_Closed by @tjkuson on 2023-09-13 17:13_

---

_Comment by @charliermarsh on 2023-09-13 17:15_

No worries at all, thanks for filing.

---
