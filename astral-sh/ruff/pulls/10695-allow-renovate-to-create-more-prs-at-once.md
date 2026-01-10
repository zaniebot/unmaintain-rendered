```yaml
number: 10695
title: Allow renovate to create more PRs at once
type: pull_request
state: merged
author: AlexWaygood
labels: []
assignees: []
merged: true
base: main
head: renovate-rate-limiting
created_at: 2024-04-01T11:34:19Z
updated_at: 2024-04-01T11:43:09Z
url: https://github.com/astral-sh/ruff/pull/10695
synced_at: 2026-01-10T22:47:02Z
```

# Allow renovate to create more PRs at once

---

_Pull request opened by @AlexWaygood on 2024-04-01 11:34_

## Summary

By default, apparently renovate will only create 2 PRs an hour, on the assumption that more than that would be overwhelming (especially when onboarding a repository to renovate): https://docs.renovatebot.com/configuration-options/#prhourlylimit. That's not really what we want here, though.

## Test Plan

I validated the config using renovate's CLI tool:

```
(renovate-rate-limiting)⚡ % npx --yes --package renovate -- renovate-config-validator                                                  ~/dev/ruff
(node:80789) [DEP0040] DeprecationWarning: The `punycode` module is deprecated. Please use a userland alternative instead.
(Use `node --trace-deprecation ...` to show where the warning was created)
 INFO: Validating .github/renovate.json5
 INFO: Config validated successfully
```

---

_Merged by @AlexWaygood on 2024-04-01 11:43_

---

_Closed by @AlexWaygood on 2024-04-01 11:43_

---

_Branch deleted on 2024-04-01 11:43_

---
