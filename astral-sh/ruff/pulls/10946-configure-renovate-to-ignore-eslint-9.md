```yaml
number: 10946
title: Configure Renovate to ignore ESLint 9
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: ignore-eslint-9
created_at: 2024-04-15T06:38:12Z
updated_at: 2024-04-15T06:52:44Z
url: https://github.com/astral-sh/ruff/pull/10946
synced_at: 2026-01-12T15:55:33Z
```

# Configure Renovate to ignore ESLint 9

---

_@MichaReiser_

## Summary

^

## Test Plan

❯ npx --yes --package renovate -- renovate-config-validator --strict
 INFO: Validating .github/renovate.json5
 INFO: Config validated successfully



---

_Label `ci` added by @MichaReiser on 2024-04-15 06:38_

---

_Review comment by @dhruvmanila on `.github/renovate.json5`:58 on 2024-04-15 06:41_

I'm not sure what the syntax is here but I was wondering if this will restrict other package which contains "eslint" in it like "typescript-eslint". I assume not.

---

_@dhruvmanila approved on 2024-04-15 06:41_

---

_Merged by @MichaReiser on 2024-04-15 06:52_

---

_Closed by @MichaReiser on 2024-04-15 06:52_

---

_Branch deleted on 2024-04-15 06:52_

---
