```yaml
number: 1697
title: Require explicit opt-in for GitHub and Gitlab formats
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/explicit
created_at: 2023-01-06T20:02:50Z
updated_at: 2023-01-07T17:15:34Z
url: https://github.com/astral-sh/ruff/pull/1697
synced_at: 2026-01-12T15:55:06Z
```

# Require explicit opt-in for GitHub and Gitlab formats

---

_@charliermarsh_

_No description provided._

---

_Comment by @charliermarsh on 2023-01-06 20:03_

\cc @edgarrmondragon - Does this seem reasonable to you? I think folks will find it surprising that this behavior happens automatically, rather than via explicit configuration or a dedicated action.

---

_Comment by @edgarrmondragon on 2023-01-06 20:37_

> \cc @edgarrmondragon - Does this seem reasonable to you? I think folks will find it surprising that this behavior happens automatically, rather than via explicit configuration or a dedicated action.

@charliermarsh Yeah, I was following what other tools do (e.g. https://github.com/pytest-dev/pytest-github-actions-annotate-failures) but it makes sense to be explicit.

---

_Merged by @charliermarsh on 2023-01-06 20:57_

---

_Closed by @charliermarsh on 2023-01-06 20:57_

---

_Branch deleted on 2023-01-06 20:57_

---

_Comment by @ashb on 2023-01-07 17:13_

Is there any way I can drive this by an env var?

My setup is I have a `poetry run ruff` in my `.pre-commit.yaml` file. and it would be nice if I could set something like `RUFF_FORMAT=github` or similar to choose the mode. Is this possible? (I couldn't see something like in the docs or a quick skim of the code)

---

_Comment by @charliermarsh on 2023-01-07 17:15_

It's not supported right now, but I agree that it'd be a useful thing to respect an env var here for that reason. Will add.

---
