```yaml
number: 3038
title: Minor improvements to renovate config
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate-fix
created_at: 2024-04-15T17:40:51Z
updated_at: 2024-04-15T17:50:54Z
url: https://github.com/astral-sh/uv/pull/3038
synced_at: 2026-01-12T16:05:23Z
```

# Minor improvements to renovate config

---

_@AlexWaygood_

## Summary

This is an identical PR to https://github.com/astral-sh/ruff/pull/10957.

In https://github.com/astral-sh/uv/commit/345b767874fc736639b2627bda09622252242e81, I gave renovate more time to file PRs over the course of Monday. That seemed like a good idea at the time as it didn't seem like renovate was able to get all the PRs up in time in the slot we were giving it to do its work. In actual fact, 4 hours is plenty of time for renovate to get all the weekly PRs up as long as it isn't rate-limited to 2 PRs per hour. I _also_ removed the rate limiting in https://github.com/astral-sh/uv/commit/345b767874fc736639b2627bda09622252242e81, so we can revert the change to widen the slot. This will mean that we'll only get renovate PRs between 12am and 4am rather than continuously throughout Monday.

This PR also switches off "semantic commits" (the `(fix)` or `(chore)` prefixes to PR titles), because I find them really annoying and we don't usually use them for our PRs written by humans.

## Test Plan

```
% npx --yes --package renovate -- renovate-config-validator --strict                                                                                                                    ~/dev/uv
(node:56829) [DEP0040] DeprecationWarning: The `punycode` module is deprecated. Please use a userland alternative instead.
(Use `node --trace-deprecation ...` to show where the warning was created)
 INFO: Validating .github/renovate.json5
 INFO: Config validated successfully
```


---

_Label `internal` added by @AlexWaygood on 2024-04-15 17:40_

---

_Merged by @AlexWaygood on 2024-04-15 17:50_

---

_Closed by @AlexWaygood on 2024-04-15 17:50_

---

_Branch deleted on 2024-04-15 17:50_

---
