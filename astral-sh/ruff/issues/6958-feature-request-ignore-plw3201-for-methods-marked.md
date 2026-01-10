```yaml
number: 6958
title: "Feature request: ignore `PLW3201` for methods marked with `@override`"
type: issue
state: closed
author: Avasam
labels:
  - good first issue
  - accepted
assignees: []
created_at: 2023-08-28T21:31:50Z
updated_at: 2023-09-12T11:54:42Z
url: https://github.com/astral-sh/ruff/issues/6958
synced_at: 2026-01-10T11:09:49Z
```

# Feature request: ignore `PLW3201` for methods marked with `@override`

---

_Issue opened by @Avasam on 2023-08-28 21:31_

Same request and reasoning as #3910

Python 3.12 will introduce the `override` decorator. It is already backported by `typing_extensions`.

Overridden method names are out of the dev's control when subclassing an external library. An example of this are Enum's `_generate_next_value_`. Ruff could understand that there's nothing the dev can do about the names if a method is marked with `@override` (and it'll still raise anyway on the base class if it's internal).

---

_Renamed from "Feature request: ignore `_generate_next_value_` for methods marked with `@override`" to "Feature request: ignore `PLW3201` for methods marked with `@override`" by @Avasam on 2023-08-28 21:32_

---

_Label `good first issue` added by @zanieb on 2023-08-28 21:34_

---

_Label `accepted` added by @zanieb on 2023-08-28 21:34_

---

_Comment by @brendonh8 on 2023-09-02 17:58_

I can take this if nobody has yet

---

_Comment by @charliermarsh on 2023-09-02 18:58_

Go for it @brendonh8

---

_Assigned to @brendonh8 by @charliermarsh on 2023-09-02 18:58_

---

_Closed by @charliermarsh on 2023-09-12 11:54_

---
