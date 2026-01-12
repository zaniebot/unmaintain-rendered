```yaml
number: 10249
title: "Fix `ruff-action` documentation to consistently use `args` instead of `options`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - documentation
assignees: []
merged: true
base: main
head: ruff-action-example
created_at: 2024-03-06T16:20:13Z
updated_at: 2024-03-06T17:09:46Z
url: https://github.com/astral-sh/ruff/pull/10249
synced_at: 2026-01-12T15:55:31Z
```

# Fix `ruff-action` documentation to consistently use `args` instead of `options`

---

_@MichaReiser_

## Summary
Fixes https://github.com/astral-sh/ruff/issues/10246

I also fixed the `args` in the example to include `check` because setting `args` replaces the default `check` args.





---

_Review comment by @charliermarsh on `docs/integrations.md`:6 on 2024-03-06 16:22_

Is this the IntelliJ thing where it bumps the word _before_ the link to the next line for unknown reasons?

---

_@charliermarsh reviewed on 2024-03-06 16:22_

---

_@charliermarsh reviewed on 2024-03-06 16:23_

---

_Review comment by @charliermarsh on `docs/integrations.md`:6 on 2024-03-06 16:23_

Can we revert the formatting changes?

---

_@MichaReiser reviewed on 2024-03-06 16:28_

---

_Review comment by @MichaReiser on `docs/integrations.md`:6 on 2024-03-06 16:28_

Oh why did it do this. I disabled markdown formatting more than once because it's supper intrusive. Sorry for that

---

_Label `documentation` added by @MichaReiser on 2024-03-06 16:29_

---

_@charliermarsh approved on 2024-03-06 17:08_

Thanks!

---

_Merged by @MichaReiser on 2024-03-06 17:09_

---

_Closed by @MichaReiser on 2024-03-06 17:09_

---

_Branch deleted on 2024-03-06 17:09_

---
