```yaml
number: 16075
title: Revert tailwindcss v4 update
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - playground
assignees: []
merged: true
base: main
head: dhruv/revert-tailwindcss-v4
created_at: 2025-02-10T12:37:47Z
updated_at: 2025-02-10T12:43:35Z
url: https://github.com/astral-sh/ruff/pull/16075
synced_at: 2026-01-12T15:55:53Z
```

# Revert tailwindcss v4 update

---

_@dhruvmanila_

## Summary

Revert the v4 update for now until the codebase is updated (https://github.com/astral-sh/ruff/pull/16069).

Update renovate config to disable updating it.

## Test Plan

```console
$ npx --yes --package renovate -- renovate-config-validator
(node:98977) [DEP0040] DeprecationWarning: The `punycode` module is deprecated. Please use a userland alternative instead.
(Use `node --trace-deprecation ...` to show where the warning was created)
 INFO: Validating .github/renovate.json5
 INFO: Config validated successfully
```

And run `npm run build` in the `playground/` directory.


---

_Label `internal` added by @dhruvmanila on 2025-02-10 12:37_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-02-10 12:37_

---

_@AlexWaygood approved on 2025-02-10 12:41_

---

_Label `playground` added by @AlexWaygood on 2025-02-10 12:41_

---

_Merged by @dhruvmanila on 2025-02-10 12:43_

---

_Closed by @dhruvmanila on 2025-02-10 12:43_

---

_Branch deleted on 2025-02-10 12:43_

---
