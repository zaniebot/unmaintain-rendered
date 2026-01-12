```yaml
number: 706
title: "Add `extend-immutable-calls` setting for B008"
type: pull_request
state: merged
author: edgarrmondragon
labels: []
assignees: []
merged: true
base: main
head: feat/b008-extend-immutable-calls
created_at: 2022-11-12T20:03:15Z
updated_at: 2022-11-21T11:08:09Z
url: https://github.com/astral-sh/ruff/pull/706
synced_at: 2026-01-12T05:48:45Z
```

# Add `extend-immutable-calls` setting for B008

---

_Pull request opened by @edgarrmondragon on 2022-11-12 20:03_

Part of #389

Example:

```toml
[tool.ruff.flake8-bugbear]
extend-immutable-calls = ["fastapi.Depends", "fastapi.Query"]
```


---

_@charliermarsh reviewed on 2022-11-12 20:15_

---

_Review comment by @charliermarsh on `resources/test/fixtures/B008_extended.py`:16 on 2022-11-12 20:15_

I feel like this should work / be considered ok. What do you think? (It doesn’t have to be part of this PR, just wondering if it’s _desired_ that it’s not accepted.)

---

_@charliermarsh reviewed on 2022-11-12 20:25_

---

_Review comment by @charliermarsh on `resources/test/fixtures/B008_extended.py`:16 on 2022-11-12 20:25_

(I want to refactor some of the import-from tracking anyway.)

---

_Merged by @charliermarsh on 2022-11-12 20:48_

---

_Closed by @charliermarsh on 2022-11-12 20:48_

---

_Branch deleted on 2022-11-12 21:34_

---

_@JonathanPlasse reviewed on 2022-11-21 11:08_

---

_Review comment by @JonathanPlasse on `resources/test/fixtures/B008_extended.py`:16 on 2022-11-21 11:08_

`convert_exit_to_sys_exit.get_member_import_name_alias()` could for this.
I can look into it.

---
