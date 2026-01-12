```yaml
number: 356
title: Warn the user if an explicitly selected check code is ignored
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/warn
created_at: 2022-10-07T21:33:10Z
updated_at: 2022-10-07T21:41:41Z
url: https://github.com/astral-sh/ruff/pull/356
synced_at: 2026-01-12T05:48:45Z
```

# Warn the user if an explicitly selected check code is ignored

---

_Pull request opened by @charliermarsh on 2022-10-07 21:33_

Resolves #355.

---

_Renamed from "Warn the user if they attempt to enable an ignored code" to "Warn the user if they explicitly select a check code that's ultimately ignored" by @charliermarsh on 2022-10-07 21:33_

---

_Renamed from "Warn the user if they explicitly select a check code that's ultimately ignored" to "Warn the user if an explicitly selected check code is ignored" by @charliermarsh on 2022-10-07 21:33_

---

_Merged by @charliermarsh on 2022-10-07 21:36_

---

_Closed by @charliermarsh on 2022-10-07 21:36_

---

_Branch deleted on 2022-10-07 21:36_

---

_Comment by @adriangb on 2022-10-07 21:36_

Just playing devils advocate here: let’s assume I’m a user doing this on purpose. I think more than likely it would be some sort of manual exploratory work (like “how many of these errors are there? maybe I can just fix em…). I think a warning in that scenario is okay. And to make it permanent you’d just delete the ignore from the config. So 👍 this makes sense.

---

_Comment by @charliermarsh on 2022-10-07 21:41_

Ah good question!

This should actually only trigger in cases in which the user passes in an error code via `--select` or `--extend-select`, and it gets nullified completely by an ignore (either `--ignore` or `--extend-ignore` or from the `pyproject.toml`).

If a user wants to test out an error that is currently ignored in `pyproject.toml`, then they do need to override that ignore somehow (e.g., remove it from the config) in order for it to be enforced at all.  So I think the warning is still right in that case: it only shows up if you `--select` something, but the other settings combine such that your `--select` actually didn't do anything.


---

_Comment by @charliermarsh on 2022-10-07 21:41_

(Alternatively, if someone passes a code to `--select`, maybe we should override the `--ignore` completely. I don't see Flake8 doing that, but it's an option.)

---
