```yaml
number: 1998
title: "Support for `--format=freeze` and `--format=json` in `uv pip list`"
type: pull_request
state: merged
author: sbrugman
labels: []
assignees: []
merged: true
base: main
head: pip-list-format
created_at: 2024-02-26T22:56:53Z
updated_at: 2024-03-06T09:31:23Z
url: https://github.com/astral-sh/uv/pull/1998
synced_at: 2026-01-12T16:04:49Z
```

# Support for `--format=freeze` and `--format=json` in `uv pip list`

---

_@sbrugman_

Implements `pip list --format=freeze` and `pip list --format=json`

Closes https://github.com/astral-sh/uv/issues/1970

## Test Plan

Extended existing `pip list` tests to match output.
Need to look at escaping in the Windows test 🪟 

---

_@T-256 reviewed on 2024-02-27 10:41_

---

_Review comment by @T-256 on `crates/uv/tests/pip_list.rs`:723 on 2024-02-27 10:41_

I think most of _freeze_ output format's usecase is to export and re-install current packages in new environment (!?)

Do you know what's usecase of showing version of editable instead of url?

Could we have `pip freeze` behavior on here and show url? Like:
`-e "../../scripts/editable-installs/poetry_editable"`

---

_@sbrugman reviewed on 2024-02-27 12:27_

---

_Review comment by @sbrugman on `crates/uv/tests/pip_list.rs`:723 on 2024-02-27 12:27_

Users will expect uv pip list to behave the same as pip list, so unless there is good reason I would not deviate.

Perhaps there are relevant discussions on the pip repo?

---

_@T-256 reviewed on 2024-02-28 13:42_

---

_Review comment by @T-256 on `crates/uv/tests/pip_list.rs`:723 on 2024-02-28 13:42_

> Users will expect uv pip list to behave the same as pip list, so unless there is good reason I would not deviate.

Actually, I don't see reasons and usacases of current `--format freeze` doesn't show urls.
Instead of making overhead, I prefer `list --format freeze` take precedence of `freeze` command over time.


> Perhaps there are relevant discussions on the pip repo?

Yes, there should. needs investigation.

Also, I think it's good to considering this as part of #2023 

---

_@sbrugman reviewed on 2024-02-28 15:05_

---

_Review comment by @sbrugman on `crates/uv/tests/pip_list.rs`:723 on 2024-02-28 15:05_

One resource to check if users rely on the current behaviour could be:
https://grep.app/search?q=pip%20freeze

---

_@charliermarsh approved on 2024-03-06 00:56_

Thanks! Sorry for the delay.

---

_Merged by @charliermarsh on 2024-03-06 01:46_

---

_Closed by @charliermarsh on 2024-03-06 01:46_

---

_Branch deleted on 2024-03-06 09:31_

---
