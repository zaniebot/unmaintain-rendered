```yaml
number: 10957
title: Minor improvements to renovate config
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
assignees: []
merged: true
base: main
head: renovate-fix
created_at: 2024-04-15T17:24:40Z
updated_at: 2024-04-15T17:38:02Z
url: https://github.com/astral-sh/ruff/pull/10957
synced_at: 2026-01-10T22:37:01Z
```

# Minor improvements to renovate config

---

_Pull request opened by @AlexWaygood on 2024-04-15 17:24_

## Summary

In https://github.com/astral-sh/ruff/commit/2ea0c3dce6412990d522e262828a247c746d9e07, I gave renovate more time to file PRs over the course of Monday. That seemed like a good idea at the time as it didn't seem like renovate was able to get all the PRs up in time in the slot we were giving it to do its work. In actual fact, 4 hours is plenty of time for renovate to get all the weekly PRs up as long as it isn't rate-limited to 2 PRs per hour. Following https://github.com/astral-sh/ruff/commit/8d547ef83a85a5cabe749a4cf09a41f931dc8116, we can revert https://github.com/astral-sh/ruff/commit/2ea0c3dce6412990d522e262828a247c746d9e07, which will mean that we'll only get renovate PRs between 12am and 4am rather than continuously throughout Monday.

This PR also switches off "semantic commits" (the `(fix)` or `(chore)` prefixes to PR titles), because I find them really annoying and we don't usually use them for our PRs written by humans.

## Test Plan

```
% npx --yes --package renovate -- renovate-config-validator --strict                                                                                                                    ~/dev/ruff
(node:56333) [DEP0040] DeprecationWarning: The `punycode` module is deprecated. Please use a userland alternative instead.
(Use `node --trace-deprecation ...` to show where the warning was created)
 INFO: Validating .github/renovate.json5
 INFO: Config validated successfully
```


---

_Label `internal` added by @AlexWaygood on 2024-04-15 17:24_

---

_@MichaReiser approved on 2024-04-15 17:28_

---

_Label `ci` added by @MichaReiser on 2024-04-15 17:29_

---

_Label `internal` removed by @MichaReiser on 2024-04-15 17:29_

---

_Merged by @AlexWaygood on 2024-04-15 17:33_

---

_Closed by @AlexWaygood on 2024-04-15 17:33_

---

_Branch deleted on 2024-04-15 17:38_

---
