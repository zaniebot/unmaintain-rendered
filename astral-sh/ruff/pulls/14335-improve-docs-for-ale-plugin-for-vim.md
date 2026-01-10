```yaml
number: 14335
title: Improve docs for ALE plugin for vim
type: pull_request
state: merged
author: pgiraud
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-11-14T10:55:40Z
updated_at: 2024-11-14T13:06:30Z
url: https://github.com/astral-sh/ruff/pull/14335
synced_at: 2026-01-10T20:50:57Z
```

# Improve docs for ALE plugin for vim

---

_Pull request opened by @pgiraud on 2024-11-14 10:55_

2 different fixers are available in ALE :
 - ruff which runs `ruff check --fix` command (useful for example when isort is enabled in lint config),
 - ruff_format which runs `run format` command.

The documentation was missing `ruff` as a possible fixer in ALE.

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-11-14 11:08_

---

_Label `documentation` added by @MichaReiser on 2024-11-14 11:08_

---

_@dhruvmanila approved on 2024-11-14 11:13_

Thanks! For reference, here's the fixer: https://github.com/dense-analysis/ale/blob/master/autoload/ale/fixers/ruff.vim

---

_@dhruvmanila reviewed on 2024-11-14 11:14_

---

_Review comment by @dhruvmanila on `docs/editors/setup.md`:156 on 2024-11-14 11:14_

Let's update the comment to state that `ruff` is for fixing all auto-fixable problems while `ruff_format` runs the formatter

---

_@pgiraud reviewed on 2024-11-14 11:32_

---

_Review comment by @pgiraud on `docs/editors/setup.md`:156 on 2024-11-14 11:32_

Additional information added.

---

_Merged by @dhruvmanila on 2024-11-14 13:01_

---

_Closed by @dhruvmanila on 2024-11-14 13:01_

---

_Branch deleted on 2024-11-14 13:06_

---
