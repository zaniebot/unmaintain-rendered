```yaml
number: 3782
title: "Adding ability to choose certain codes to error on (instead of disabling all using `--exit-zero`) or not error on"
type: issue
state: closed
author: JSpenced
labels: []
assignees: []
created_at: 2023-03-28T18:49:41Z
updated_at: 2023-04-13T03:39:01Z
url: https://github.com/astral-sh/ruff/issues/3782
synced_at: 2026-01-12T15:54:44Z
```

# Adding ability to choose certain codes to error on (instead of disabling all using `--exit-zero`) or not error on

---

_@JSpenced_

This is more a feature request or maybe I missed something in the docs to enable it.

From what I see, currently, I have `--exit-zero` in our `pre-commit` file, but we would like it to output all linting errors that we have included. Then only error on certain ones in the `pre-commit`. I know something similar is possible using `fixable` and `unfixable`, but not for which codes to error on or not.

Does this feature seem useful and possibly will be implemented in the future?

---

_Comment by @charliermarsh on 2023-03-28 23:34_

Yeah I think this could in theory be tackled by adding "severities" (so that users can differentiate errors from warnings), so I'm gonna close in favor of https://github.com/charliermarsh/ruff/issues/1256.

---

_Closed by @charliermarsh on 2023-03-28 23:34_

---

_Comment by @JSpenced on 2023-04-13 03:36_

Yup 💯 ! Thanks for the link.

---

_Comment by @charliermarsh on 2023-04-13 03:39_

Any time :) Links are easy, it's changes that take work :joy:

---
