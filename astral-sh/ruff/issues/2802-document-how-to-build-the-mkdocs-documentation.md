```yaml
number: 2802
title: Document how to build the mkdocs documentation
type: issue
state: closed
author: not-my-profile
labels: []
assignees: []
created_at: 2023-02-12T05:02:48Z
updated_at: 2023-02-12T16:07:26Z
url: https://github.com/astral-sh/ruff/issues/2802
synced_at: 2026-01-12T15:54:43Z
```

# Document how to build the mkdocs documentation

---

_@not-my-profile_

`CONTRIBUTING.md` should explain how you can build our mkdocs documentation so that you can preview your local changes.

---

_Renamed from "Document how to build the documentation" to "Document how to build the mkdocs documentation" by @not-my-profile on 2023-02-12 05:36_

---

_Comment by @trag1c on 2023-02-12 06:02_

I can do this one :)

---

_Comment by @not-my-profile on 2023-02-12 06:08_

Just a note: In particular I am talking about the dependencies that are needed ... these are currently only specified in the GitHub workflow file:

https://github.com/charliermarsh/ruff/blob/0123425be15a6b09f22bd23489c648e534fd9197/.github/workflows/docs.yaml#L18

These should instead be defined in e.g. `docs/requirements.txt` ... and the above command should be changed to a `pip install -r docs/requirements.txt`. And then the `CONTRIBUTING.md` file could tell you to run that same command.

---

_Comment by @trag1c on 2023-02-12 06:12_

@not-my-profile Any suggestions on where exactly that section should be placed? 🤔 

---

_Comment by @not-my-profile on 2023-02-12 06:14_

Maybe before *Release process*.

---

_Closed by @charliermarsh on 2023-02-12 16:07_

---
