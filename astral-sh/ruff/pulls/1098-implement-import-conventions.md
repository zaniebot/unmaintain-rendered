```yaml
number: 1098
title: Implement import conventions
type: pull_request
state: merged
author: edgarrmondragon
labels: []
assignees: []
merged: true
base: main
head: 1004-import-conventions
created_at: 2022-12-06T02:55:05Z
updated_at: 2022-12-06T21:05:43Z
url: https://github.com/astral-sh/ruff/pull/1098
synced_at: 2026-01-12T05:36:31Z
```

# Implement import conventions

---

_Pull request opened by @edgarrmondragon on 2022-12-06 02:55_

Closes #1004 

---

_Marked ready for review by @edgarrmondragon on 2022-12-06 05:46_

---

_Comment by @JonathanPlasse on 2022-12-06 08:23_

Maybe add a test that overrides a default conventional alias (e.g. `"numpy" = "nmp"` instead of `np`) and an other ones that remove a default conventional alias (e.g. `"seaborn": ""` would remove from the map `"seaborn": "sns"`).

---

_@charliermarsh reviewed on 2022-12-06 14:43_

---

_Review comment by @charliermarsh on `README.md`:793 on 2022-12-06 14:43_

Do you think it'd be a huge mistake to use a different prefix here? I'm trying to move towards using three-letter prefixes for all plugins, both for consistency and to avoid collisions in the future. We're already deviating on the error code a bit (since the plugin has IC002 and such), so maybe it's fine? Like `ICO`?

---

_@charliermarsh reviewed on 2022-12-06 14:43_

---

_Review comment by @charliermarsh on `README.md`:793 on 2022-12-06 14:43_

Do you think it'd be a huge mistake to use a different prefix here? I'm trying to move towards using three-letter prefixes for all plugins, both for consistency and to avoid collisions in the future. We're already deviating on the error code a bit (since the plugin has IC002 and such), so maybe it's fine? Like `ICO` or `ICN`?

---

_Review comment by @edgarrmondragon on `README.md`:793 on 2022-12-06 16:36_

No, I think it's perfectly fine. I'll go with `ICO` 👍 

---

_@edgarrmondragon reviewed on 2022-12-06 16:36_

---

_Comment by @edgarrmondragon on 2022-12-06 16:40_

> Maybe add a test that overrides a default conventional alias (e.g. `"numpy" = "nmp"` instead of `np`) and an other ones that remove a default conventional alias (e.g. `"seaborn": ""` would remove from the map `"seaborn": "sns"`).

@JonathanPlasse Ah, so you're thinking the configured aliases would _extend_ the default ones instead of completely _overriding_ them. That makes sense, so users wouldn't have to re-declare `"pandas" = "pd"` if they just want to add a new alias. I'll work on that approach.

---

_Review comment by @charliermarsh on `README.md`:1778 on 2022-12-06 17:05_

I think most similar to other setups would be to have `aliases` and `extend-aliases` (which defaults to `[]`, but is concatenated to `aliases`).

---

_@charliermarsh reviewed on 2022-12-06 17:05_

---

_@edgarrmondragon reviewed on 2022-12-06 19:49_

---

_Review comment by @edgarrmondragon on `README.md`:793 on 2022-12-06 19:49_

Though `ICO001` can be ambiguous with `IC0001` in some fonts

---

_@charliermarsh reviewed on 2022-12-06 20:26_

---

_Review comment by @charliermarsh on `README.md`:1778 on 2022-12-06 20:26_

@edgarrmondragon - I can tweak this and merge if you agree?

---

_@charliermarsh reviewed on 2022-12-06 20:32_

---

_Review comment by @charliermarsh on `src/flake8_import_conventions/checks.rs`:7 on 2022-12-06 20:32_

Do you prefer `ICO` or `ICN`? I see `ICN` in the enums but `ICO` here :)

---

_@charliermarsh reviewed on 2022-12-06 20:38_

---

_Review comment by @charliermarsh on `README.md`:1778 on 2022-12-06 20:38_

Oh nm, you're all over it :)

---

_@edgarrmondragon reviewed on 2022-12-06 20:38_

---

_Review comment by @edgarrmondragon on `src/flake8_import_conventions/checks.rs`:7 on 2022-12-06 20:38_

I ended up preferring `ICN` to avoid ambiguity in some fonts that might make `0` and `O` hard to tell apart

---

_Merged by @charliermarsh on 2022-12-06 21:01_

---

_Closed by @charliermarsh on 2022-12-06 21:01_

---

_Comment by @charliermarsh on 2022-12-06 21:01_

Rock, thank you @edgarrmondragon! Made a couple changes at the end but LMK if you disagree w/ any of them!

---

_Branch deleted on 2022-12-06 21:05_

---

_Branch restored on 2022-12-06 21:05_

---

_Branch deleted on 2022-12-06 21:05_

---

_Branch restored on 2022-12-06 21:05_

---
