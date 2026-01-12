```yaml
number: 3016
title: "Avoid raising `B027` violations in `.pyi` files"
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: B027-false-positive
created_at: 2023-02-18T17:37:42Z
updated_at: 2023-02-19T17:26:22Z
url: https://github.com/astral-sh/ruff/pull/3016
synced_at: 2026-01-12T15:55:12Z
```

# Avoid raising `B027` violations in `.pyi` files

---

_@JonathanPlasse_

- Close #3012 

Should `is_stub` be renamed to `is_pyi`?

---

_Renamed from "B027 false positive" to "Avoid raising `B027` violations in `.pyi` files" by @charliermarsh on 2023-02-19 14:19_

---

_Merged by @charliermarsh on 2023-02-19 14:21_

---

_Closed by @charliermarsh on 2023-02-19 14:21_

---

_Branch deleted on 2023-02-19 14:53_

---

_Comment by @JonathanPlasse on 2023-02-19 17:22_

Where does the term `interface_definition` come from?
I never came across it.

---

_Comment by @charliermarsh on 2023-02-19 17:26_

https://github.com/python/mypy/issues/345#issuecomment-51003735

---

_Comment by @charliermarsh on 2023-02-19 17:26_

I believe `.pyi` is roughly "Python interface"

---
