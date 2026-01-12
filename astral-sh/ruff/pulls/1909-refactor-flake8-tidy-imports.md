```yaml
number: 1909
title: "Refactor `flake8_tidy_imports`"
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: refactor-flake8_tidy_imports
created_at: 2023-01-16T07:57:26Z
updated_at: 2023-01-16T16:27:24Z
url: https://github.com/astral-sh/ruff/pull/1909
synced_at: 2026-01-12T15:55:07Z
```

# Refactor `flake8_tidy_imports`

---

_@not-my-profile_

This PR kicks off our larger refactoring effort which entails:

* breaking up the `rules.rs` files into one file per rule (see also [#1781](https://www.github.com/charliermarsh/ruff/issues/1781))
* moving the tests from `mod.rs` to these new files
* moving the Violation structs & impls from `violations.rs` to these new files

Ultimately I also think the rule-specific options should be defined in the rule-specific files but to do that I'll firstly have to improve our `ConfigurationOptions` derive macro.

---

_Merged by @charliermarsh on 2023-01-16 16:27_

---

_Closed by @charliermarsh on 2023-01-16 16:27_

---
