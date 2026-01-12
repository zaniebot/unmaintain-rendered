```yaml
number: 996
title: "Implement F811 (\"redefinition of unused name from line N\")"
type: issue
state: closed
author: charliermarsh
labels:
  - rule
assignees: []
created_at: 2022-12-02T14:25:04Z
updated_at: 2022-12-09T02:31:10Z
url: https://github.com/astral-sh/ruff/issues/996
synced_at: 2026-01-12T15:54:40Z
```

# Implement F811 ("redefinition of unused name from line N")

---

_@charliermarsh_

_No description provided._

---

_Label `rule` added by @charliermarsh on 2022-12-02 14:25_

---

_Comment by @LefterisJP on 2022-12-04 11:14_

Question charlie. Is my understanding correct that this is the last flake8 rule to not exist in ruff? So one could completely replace flake8 with ruff once this is done? If yes I am watching this one :smile: 

---

_Comment by @charliermarsh on 2022-12-04 14:51_

@LefterisJP - Yeah this is the last Flake8 rule! (Assuming you use Black, which makes a bunch of the pycodestyle rules around spacing and such redundant.)

---

_Comment by @LefterisJP on 2022-12-04 17:03_

> @LefterisJP - Yeah this is the last Flake8 rule! (Assuming you use Black, which makes a bunch of the pycodestyle rules around spacing and such redundant.)

We dont't use black unfortunately. I am in the minority that can't stand black and am still looking for a code styler that is a bit more customizable to make it enforce the style we have at rotki.

yapf (https://github.com/google/yapf) comes close to that but still some rules it does not check/for enforce yet. Do you know of any other?

---

_Comment by @charliermarsh on 2022-12-04 17:28_

Sounds like you need ruff fmt!

---

_Comment by @LefterisJP on 2022-12-04 18:04_

> Sounds like you need ruff fmt!

To be honest that would be cool. I am looking for something like what clangformat is for C/C++ projects.

---

_Comment by @charliermarsh on 2022-12-04 18:06_

It's on my roadmap. I kind of want the customizability of `rustfmt` (never used `clangformat` so hard for me to compare).


---

_Comment by @charliermarsh on 2022-12-04 22:23_

(I will get to this soon, it does add complexity but is hopefully worth the milestone...)

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-04 22:23_

---

_Closed by @charliermarsh on 2022-12-09 02:31_

---
