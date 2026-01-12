```yaml
number: 383
title: Pre-commit repo is not updated with new releases
type: issue
state: closed
author: ghost
labels: []
assignees: []
created_at: 2022-10-10T08:04:46Z
updated_at: 2022-10-12T15:19:39Z
url: https://github.com/astral-sh/ruff/issues/383
synced_at: 2026-01-12T15:54:40Z
```

# Pre-commit repo is not updated with new releases

---

_@ghost_

pre-commit version is only 0.0.48
https://github.com/charliermarsh/ruff-pre-commit

---

_Renamed from "Pre-commit repo is not updated for 20 days" to "Pre-commit repo is not updated with new releases" by @ghost on 2022-10-10 08:09_

---

_Comment by @charliermarsh on 2022-10-10 13:08_

Yeah sadly I've been doing this manually. I'll update it now. But it should be automated.

---

_Label `enhancement` added by @charliermarsh on 2022-10-10 13:08_

---

_Comment by @charliermarsh on 2022-10-10 13:11_

I just published v0.0.65. Will think on how to automate it.

---

_Comment by @fsouza on 2022-10-12 01:55_

@charliermarsh I can send a PR with the automation. It's basically what I have here: https://github.com/fsouza/mirrors-ruff/blob/main/.github/workflows/main.yml

It leverages `pre-commit-mirror-maker`, created by the creator of pre-commit.

---

_Comment by @charliermarsh on 2022-10-12 01:57_

Oh cool, that would be much appreciated!

---

_Comment by @charliermarsh on 2022-10-12 15:19_

This is fixed thanks to some great work from @fsouza.

---

_Closed by @charliermarsh on 2022-10-12 15:19_

---
