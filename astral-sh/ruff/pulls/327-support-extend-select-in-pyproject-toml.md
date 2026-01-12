```yaml
number: 327
title: Support extend-select in pyproject.toml
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: extend-select
created_at: 2022-10-04T20:29:23Z
updated_at: 2022-10-04T22:33:23Z
url: https://github.com/astral-sh/ruff/pull/327
synced_at: 2026-01-12T15:55:04Z
```

# Support extend-select in pyproject.toml

---

_@andersk_

(I didn’t add `extend-ignore` since it’s the same as `ignore`—should we remove the `--extend-ignore` command line option?)

---

_@charliermarsh reviewed on 2022-10-04 21:23_

---

_Review comment by @charliermarsh on `src/settings.rs`:174 on 2022-10-04 21:23_

I feel like the semantics here are off (but that they were off before with `settings.ignore(&config.ignore)` -- not any fault of your change). We should probably avoid calling `settings.ignore(&config.ignore)`, since if the user provides a `--ignore` on the command-line, we'd want to respect that _instead_ (not in addition). Same for `extend_select`.

But this can be changed separately, what you have here is an improvement in overall behavior.


---

_Comment by @charliermarsh on 2022-10-04 21:24_

I think it's conceptually nice to have both `--ignore` and `--extend-ignore`. At least on the command-line, it's useful because you could define an `ignore` in the `pyproject.toml`, then use `--extend-ignore` on the command-line to augment it.

---

_Merged by @charliermarsh on 2022-10-04 21:24_

---

_Closed by @charliermarsh on 2022-10-04 21:24_

---

_Branch deleted on 2022-10-04 22:33_

---
