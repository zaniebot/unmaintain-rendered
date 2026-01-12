```yaml
number: 836
title: Fix N804 class method with positional only args
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: fix-n804-class-method-pos-only-args
created_at: 2022-11-20T20:36:22Z
updated_at: 2022-12-01T17:15:17Z
url: https://github.com/astral-sh/ruff/pull/836
synced_at: 2026-01-12T05:48:45Z
```

# Fix N804 class method with positional only args

---

_Pull request opened by @JonathanPlasse on 2022-11-20 20:36_

_No description provided._

---

_@charliermarsh reviewed on 2022-11-20 20:38_

---

_Review comment by @charliermarsh on `src/pep8_naming/checks.rs`:87 on 2022-11-20 20:38_

Nit: can we make this an `else`, and remove the `return None` from line 84, to make the intent a bit clearer?

---

_@charliermarsh reviewed on 2022-11-20 20:41_

---

_Review comment by @charliermarsh on `resources/test/fixtures/N804.py`:35 on 2022-11-20 20:41_

(Should we add one variant where `cls` _isn't_ the first argument, but it does have positional-only args?)

---

_Merged by @charliermarsh on 2022-11-20 20:48_

---

_Closed by @charliermarsh on 2022-11-20 20:48_

---

_Branch deleted on 2022-12-01 17:15_

---
