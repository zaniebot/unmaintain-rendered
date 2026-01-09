---
number: 1208
title: "Suggestion: use PEP8-default max line length of 79"
type: issue
state: closed
author: vytas7
labels: []
assignees: []
created_at: 2022-12-12T13:46:27Z
updated_at: 2022-12-12T21:31:51Z
url: https://github.com/astral-sh/ruff/issues/1208
synced_at: 2026-01-07T13:12:14-06:00
---

# Suggestion: use PEP8-default max line length of 79

---

_Issue opened by @vytas7 on 2022-12-12 13:46_

Not really an issue _per se_ since the default of 88 is documented in README, but a discussion point.

Shouldn't `ruff` default to the same maximum line length of 79 as [specified in PEP8](https://peps.python.org/pep-0008/#maximum-line-length)?
Apart from just the sake of following PEP8, this makes `ruff` incompatible with `flake8` if using both with default settings.

---

_Comment by @charliermarsh on 2022-12-12 21:31_

I do appreciate the suggestion but I opted for 88 for consistency with Black. I don't think there's really a right answer here (though I could document it).

---

_Closed by @charliermarsh on 2022-12-12 21:31_

---
