---
number: 11877
title: Support showing Bandit issues by severity
type: issue
state: closed
author: rahult-graphcore
labels: []
assignees: []
created_at: 2024-06-14T15:01:58Z
updated_at: 2024-06-14T15:23:49Z
url: https://github.com/astral-sh/ruff/issues/11877
synced_at: 2026-01-07T13:12:15-06:00
---

# Support showing Bandit issues by severity

---

_Issue opened by @rahult-graphcore on 2024-06-14 15:01_

Bandit supports reporting on only high-severity issues using the `-lll` flag e.g.
`bandit examples/*.py -n 3 -lll`

or medium and high severity issues with `-ll`
`bandit examples/*.py -n 3 -ll`

This is quite useful when running on a large codebase to filter down to only the most serious issues.
This is a request to add this config option to Bandit in the Ruff linter.

---

_Comment by @MichaReiser on 2024-06-14 15:23_

Hi

I can see how this is useful. We're considering to introduce a warning level to distinguish between different severities. See https://github.com/astral-sh/ruff/issues/1256

I don't think we want to go as far as having more levels other than warning and error. I'll close this in favor of https://github.com/astral-sh/ruff/issues/1256

---

_Closed by @MichaReiser on 2024-06-14 15:23_

---
