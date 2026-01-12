```yaml
number: 7507
title: "Deprecate `PGH002` in favor of `G010`"
type: pull_request
state: closed
author: charliermarsh
labels:
  - rule
assignees: []
base: charlie/PGH001
head: charlie/PGH002
created_at: 2023-09-19T03:37:03Z
updated_at: 2023-11-30T22:20:45Z
url: https://github.com/astral-sh/ruff/pull/7507
synced_at: 2026-01-12T15:55:24Z
```

# Deprecate `PGH002` in favor of `G010`

---

_@charliermarsh_

## Summary

These rules are identical. `flake8-logging-format` is the more popular and more well-defined category, so we remove `PGH002` and add a redirect to `G010`. (By adding the redirect, we ensure that users see a warning but not an error when they reference `PGH002` in their configuration or in a `# noqa`, and that those codes seamlessly redirect to `G010`.)

Part of https://github.com/astral-sh/ruff/issues/7502.

---

_Label `rule` added by @charliermarsh on 2023-09-19 03:37_

---

_Review requested from @dhruvmanila by @charliermarsh on 2023-09-19 03:37_

---

_@dhruvmanila approved on 2023-09-19 03:41_

---

_Comment by @MichaReiser on 2023-09-19 06:13_

Is this a breaking change? What should we note down in the changelog?

---

_@konstin approved on 2023-09-19 11:17_

---

_Comment by @zanieb on 2023-11-30 22:19_

Related discussion re breaking change at https://github.com/astral-sh/ruff/pull/7506

---

_Comment by @zanieb on 2023-11-30 22:20_

Closing in favor of https://github.com/astral-sh/ruff/issues/8931

---

_Closed by @zanieb on 2023-11-30 22:20_

---
