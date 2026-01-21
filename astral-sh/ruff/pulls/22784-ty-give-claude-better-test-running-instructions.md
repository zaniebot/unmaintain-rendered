```yaml
number: 22784
title: "[ty] give Claude better test-running instructions"
type: pull_request
state: open
author: carljm
labels:
  - internal
assignees: []
base: main
head: cjm/claude-md
created_at: 2026-01-21T06:07:30Z
updated_at: 2026-01-21T06:07:59Z
url: https://github.com/astral-sh/ruff/pull/22784
synced_at: 2026-01-21T06:54:06Z
```

# [ty] give Claude better test-running instructions

---

_@carljm_

Claude tends to over-use `MDTEST_TEST_FILTER` when it's not necessary. It uses it for running a full mdtest file -- it's only actually needed if you want to run a single test section within an mdtest file.

This overuse is a problem because I can't get wildcards to work correctly in permission lines with `MDTEST_TEST_FILTER`, so Claude will always separately ask for permissions for each individual test file it wants to run.

Try to give it better instructions so it will stop doing that.

---

_Label `internal` added by @carljm on 2026-01-21 06:07_

---

_Review requested from @charliermarsh by @carljm on 2026-01-21 06:07_

---
