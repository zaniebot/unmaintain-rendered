```yaml
number: 1865
title: "Introduce ruff::rules module"
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: rules-module
created_at: 2023-01-14T05:34:19Z
updated_at: 2023-01-14T16:50:39Z
url: https://github.com/astral-sh/ruff/pull/1865
synced_at: 2026-01-12T05:36:32Z
```

# Introduce ruff::rules module

---

_Pull request opened by @not-my-profile on 2023-01-14 05:34_

Resolves #1547.

---

_Comment by @not-my-profile on 2023-01-14 06:15_

Huh, that's interesting apparently GitHub's file move detection is worse than git's file move detection ... if you `git show` the second commit `git` reports:

> 1247 insertions(+), 1247 deletions(-)

whereas the GitHub UI apparently just gives up and claims

> +39,491 −39,492

I therefore split up this PR into two commits so that you can separately review the first commit ... since the second commit is unfortunately pretty much unreviewable in GitHub's UI.

(But even with that GitHub limitation taken aside I think it's good to separate this change into two commits anyway.)

---

_Comment by @charliermarsh on 2023-01-14 12:28_

Cool.

Is it at all strange that we have (e.g.):

- `rules::flake8_quotes::rules` (rules twice)
- `rules::flake8_quotes::settings` (`rules` module includes settings)

Do you see this changing further in future commits?


---

_Comment by @not-my-profile on 2023-01-14 13:32_

Yes I think the duplicate "rules" could be changed to `rules::{origin}::{specific_rule}`.

I don't really mind the settings under rules since the settings are for the rules ... potentially the settings could be moved to `mod.rs` and the tests out of  `mod.rs`.

---

_Comment by @charliermarsh on 2023-01-14 15:11_

Ok cool. Do you mind rebasing, and I'll merge this next? Apologies.

---

_Comment by @not-my-profile on 2023-01-14 16:47_

Of course, no worries :) Rebased.

---

_Merged by @charliermarsh on 2023-01-14 16:48_

---

_Closed by @charliermarsh on 2023-01-14 16:48_

---

_Comment by @charliermarsh on 2023-01-14 16:48_

Thanks!

---

_Comment by @not-my-profile on 2023-01-14 16:50_

(btw I sent you an email yesterday)

---
