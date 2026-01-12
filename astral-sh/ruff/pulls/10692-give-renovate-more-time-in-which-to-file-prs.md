```yaml
number: 10692
title: Give renovate more time in which to file PRs
type: pull_request
state: merged
author: AlexWaygood
labels: []
assignees: []
merged: true
base: main
head: renovate-improve
created_at: 2024-04-01T11:03:44Z
updated_at: 2024-04-01T11:13:15Z
url: https://github.com/astral-sh/ruff/pull/10692
synced_at: 2026-01-12T15:55:33Z
```

# Give renovate more time in which to file PRs

---

_@AlexWaygood_

## Summary

It looks like renovate is currently rate-limiting us so that we can only have a few PRs open at a time, which means that giving it a 4-hour window to open all our weekly update PRs isn't really long enough

## Test Plan

I used renovate's CLI tool to validate the configuration:

```
(renovate-improve)⚡ % npx --yes --package renovate -- renovate-config-validator                                                        ~/dev/ruff
(node:78682) [DEP0040] DeprecationWarning: The `punycode` module is deprecated. Please use a userland alternative instead.
(Use `node --trace-deprecation ...` to show where the warning was created)
 INFO: Validating .github/renovate.json5
 INFO: Config validated successfully
```

---

_Merged by @AlexWaygood on 2024-04-01 11:13_

---

_Closed by @AlexWaygood on 2024-04-01 11:13_

---

_Branch deleted on 2024-04-01 11:13_

---
