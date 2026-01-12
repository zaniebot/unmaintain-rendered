```yaml
number: 3355
title: Rules mentioned in the changelog missing
type: issue
state: closed
author: xmatthias
labels:
  - question
assignees: []
created_at: 2023-03-06T05:49:22Z
updated_at: 2023-03-06T14:08:21Z
url: https://github.com/astral-sh/ruff/issues/3355
synced_at: 2026-01-12T15:54:43Z
```

# Rules mentioned in the changelog missing

---

_@xmatthias_


Version: ruff 0.0.254
the [changelog for 0.0.254](https://github.com/charliermarsh/ruff/releases/tag/v0.0.254) mentions several (great!) new rules (E211, E225, E226, E227, E228) have been added.

These seem to be missing from the documentation (they should be in the screenshot below - but also a Search doesn't find them) - and are also not applied when trying to see if they still work and are active.

Have these been disabled / removed temporarily, or is there something wrong with this / my install

![image](https://user-images.githubusercontent.com/5024695/223029056-58f7d01d-6e5b-4043-98e0-37c73f6f71d8.png)

---

_Comment by @charliermarsh on 2023-03-06 13:23_

Ah yeah, sorry about that. This is just a confusion in messaging -- we've implemented those rules, but they're currently behind a feature flag, so they're not accessible from the production build.

I want all of the remaining pycodestyle rules to go out in a single coordinated release, since it's arguably a 'breaking' change for users (it'll enable a bunch of previously non-existent rules for any users that enable the `E` category).

There are about 10 more rules to go, hopefully in the next week or two but don't hold me to it :)


---

_Closed by @charliermarsh on 2023-03-06 13:23_

---

_Label `question` added by @charliermarsh on 2023-03-06 13:23_

---

_Comment by @xmatthias on 2023-03-06 14:08_

Great to hear! 

(and thanks for this great project) 💯 

---
