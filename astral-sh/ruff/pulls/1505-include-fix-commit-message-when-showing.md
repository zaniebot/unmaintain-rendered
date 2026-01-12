```yaml
number: 1505
title: Include fix commit message when showing violations together with source
type: pull_request
state: merged
author: squiddy
labels: []
assignees: []
merged: true
base: main
head: text-report-commit-message
created_at: 2022-12-31T08:56:16Z
updated_at: 2022-12-31T12:57:20Z
url: https://github.com/astral-sh/ruff/pull/1505
synced_at: 2026-01-12T15:55:06Z
```

# Include fix commit message when showing violations together with source

---

_@squiddy_

This change doesn't have a lot of impact right now, as in many cases the commit message is similar to the error message itself. However, it is more actionable and makes it more obvious in what places we can improve.

Refs: #218

![image](https://user-images.githubusercontent.com/50333/210131029-d6439963-a7ab-4b1f-8efc-4acbc2f599ad.png)

---

I think having this new commit message is a good first step, but long term I imagine it's more useful to the user if those messages were created by the check itself. We'd have much more context there and could generate much more actionable messages, e.g. 

```
= help: replace with `x.bar`
```

---

_Comment by @charliermarsh on 2022-12-31 12:45_

Yeah I was a bit torn on this (I originally added a `message` to `Fix`, but it felt off to be adding those everywhere when the rest of the check data was associated with `Check`). I was hoping that we could accomplish it by adding more context to the enum itself (for example, in the `getattr` case, we could add the property name and value, and then use that to formulate the `commit`). We could also allow overrides on `Fix` itself.

---

_Comment by @squiddy on 2022-12-31 12:52_

Adding more context to the enum certainly is more in line to what we're doing right now. I'll play around with that a bit, adding some context to some checks, and see how that goes. :)

---

_Merged by @charliermarsh on 2022-12-31 12:54_

---

_Closed by @charliermarsh on 2022-12-31 12:54_

---

_Branch deleted on 2022-12-31 12:57_

---
