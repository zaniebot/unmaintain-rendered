```yaml
number: 1819
title: "Config option `classes` from isort (and `constants` and `variables` probably)"
type: issue
state: closed
author: tmke8
labels:
  - good first issue
  - isort
assignees: []
created_at: 2023-01-12T15:58:40Z
updated_at: 2023-01-18T12:30:53Z
url: https://github.com/astral-sh/ruff/issues/1819
synced_at: 2026-01-10T11:09:44Z
```

# Config option `classes` from isort (and `constants` and `variables` probably)

---

_Issue opened by @tmke8 on 2023-01-12 15:58_

The order in `isort` is `CONSTANTS`, `Classes`, `functions`, which is detected by the case of the letters, but sometimes classes are in all-caps, like [SVC](https://scikit-learn.org/stable/modules/generated/sklearn.svm.SVC.html) from `sklearn` or [SELU](https://pytorch.org/docs/stable/generated/torch.nn.SELU.html) from `pytorch`.

In order to sort them correctly, I can specify [classes](https://pycqa.github.io/isort/docs/configuration/options.html#classes):
```toml
[tool.isort]
classes = ["SVC", "SELU"]
```
and then `isort` treats them like classes even though they're in constant-case.

- [ ] variables
- [x] classes
- [ ] constants

---

_Label `isort` added by @charliermarsh on 2023-01-12 15:59_

---

_Label `good first issue` added by @charliermarsh on 2023-01-12 15:59_

---

_Comment by @charliermarsh on 2023-01-12 16:00_

Thanks. Not too hard to support, but I have to get to some other things first! Would be a good first contribution if anyone is interested :)

---

_Comment by @tmke8 on 2023-01-13 19:27_

I think this should have been closed by #1849.

---

_Closed by @tmke8 on 2023-01-13 19:27_

---

_Comment by @saadmk11 on 2023-01-16 12:46_

Not sure if we want to implement `constants` and `variables` options as well?

---

_Comment by @charliermarsh on 2023-01-16 16:25_

I think we should, for consistency.

---

_Reopened by @tmke8 on 2023-01-16 19:25_

---

_Closed by @charliermarsh on 2023-01-18 12:30_

---
