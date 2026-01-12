```yaml
number: 3612
title: "pre-commit is not ran in ci, resulting in possible commits that don't pass pre-commit"
type: issue
state: closed
author: onerandomusername
labels:
  - internal
assignees: []
created_at: 2023-03-19T19:36:40Z
updated_at: 2023-03-24T21:20:15Z
url: https://github.com/astral-sh/ruff/issues/3612
synced_at: 2026-01-12T15:54:43Z
```

# pre-commit is not ran in ci, resulting in possible commits that don't pass pre-commit

---

_@onerandomusername_

continuation of #3604 #3603
https://github.com/charliermarsh/ruff/blob/main/.github/workflows/ci.yaml doesn't contain an action to run pre-commit, so it only happens locally, and doesn't seem to be validated in ci.

There's two options for this, adding a job which runs pre-commit in ci, or installing https://pre-commit.ci on the repo (and perhaps adding configuration to change autoupdate)

Config will have to be added to skip the local hooks unless they aren't part of ci already.

I'm interested in adding the ci or pre-commit.ci config, for whichever route is chosen.

---

_Comment by @charliermarsh on 2023-03-19 19:53_

Yeah we're in kind of a weird state right now. Some people want pre-commit, so it exists, and there's a set of checks that _only_ run on pre-commit right now, but candidly I don't use it at all, it's not part of the contributor guidelines or getting started, and I'm hesitant to make it something contributors _have_ to install and run.

As long as it exists, though, we should at least validate it on CI, i.e., incorporate it into `ci.yaml`. We'll want to skip the steps that are already captured by our established CI workloads (I believe they're enumerated at the bottom already) -- I think those are all the local hooks anyway.


---

_Label `internal` added by @charliermarsh on 2023-03-19 19:53_

---

_Comment by @onerandomusername on 2023-03-19 19:54_

Would you rather implement it in CI or using the external plugin https://pre-commit.ci/ ?

---

_Comment by @charliermarsh on 2023-03-19 20:26_

I would rather implement it in CI.

---

_Comment by @JonathanPlasse on 2023-03-19 20:27_

Can I work on it?

---

_Comment by @onerandomusername on 2023-03-19 20:40_

I would like to implement this, as said in the original issue text 🙂 

---

_Comment by @JonathanPlasse on 2023-03-19 20:49_

Sorry, go ahead.

---

_Closed by @charliermarsh on 2023-03-24 21:20_

---
