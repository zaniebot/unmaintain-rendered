---
number: 1251
title: "Feature request: isort.float-to-top"
type: issue
state: open
author: tlambert03
labels:
  - isort
assignees: []
created_at: 2022-12-15T20:14:11Z
updated_at: 2024-08-15T13:54:40Z
url: https://github.com/astral-sh/ruff/issues/1251
synced_at: 2026-01-07T13:12:14-06:00
---

# Feature request: isort.float-to-top

---

_Issue opened by @tlambert03 on 2022-12-15 20:14_

I have some code-autogeneration projects where isort's [Float To Top](https://pycqa.github.io/isort/docs/configuration/options.html#float-to-top) feature is super useful.  (as a reminder, it causes all non-indented imports to float to the top of the file)

Not sure how "invasive" this one is to implement here, but just wanted to put in a friendly feature request :)

thanks as always!

---

_Label `enhancement` added by @charliermarsh on 2022-12-15 20:16_

---

_Label `isort` added by @charliermarsh on 2022-12-15 20:16_

---

_Label `enhancement` removed by @charliermarsh on 2022-12-31 18:14_

---

_Referenced in [astral-sh/ruff#5180](../../astral-sh/ruff/pulls/5180.md) on 2023-07-17 15:06_

---

_Referenced in [astral-sh/ruff#12084](../../astral-sh/ruff/issues/12084.md) on 2024-06-28 11:51_

---

_Comment by @failable on 2024-07-31 14:23_

Any update of this? 

---

_Comment by @Yevgnen on 2024-08-15 13:54_

I miss too much this conveient function to quickly import modules and move them to the top automatically since most open source Python LS do not perform well on auto importing. I would rather import it manually but move them by `--float-to-top`. It would be nice to have it in ruff too.

---

_Referenced in [astral-sh/ruff#14243](../../astral-sh/ruff/issues/14243.md) on 2024-11-11 09:50_

---
