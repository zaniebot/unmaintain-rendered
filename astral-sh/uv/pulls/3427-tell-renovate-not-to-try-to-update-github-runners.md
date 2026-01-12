```yaml
number: 3427
title: Tell renovate not to try to update GitHub runners
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate-github-runners
created_at: 2024-05-07T15:59:02Z
updated_at: 2024-05-07T16:10:28Z
url: https://github.com/astral-sh/uv/pull/3427
synced_at: 2026-01-12T16:05:38Z
```

# Tell renovate not to try to update GitHub runners

---

_@AlexWaygood_

## Summary

An identical PR to https://github.com/astral-sh/ruff/pull/11324. Copying-and-pasting my rationale from that PR:

If we've pinned a GitHub runner to a specific version (e.g. `macos-12` rather than `macos-latest`, we've probably done so for a specific reason, so PRs like https://github.com/astral-sh/ruff/pull/11300 are just annoying. The fix for this is a bit obscure, but it appears the way to do it is to setup a package rule that only matches renovate updates which use the `github-runners` datasource, and then set `enabled: false` in that package rule.

Docs:
- https://docs.renovatebot.com/configuration-options/#matchdatasources
- https://docs.renovatebot.com/configuration-options/#enabled

## Test Plan

```
% npx --yes --package renovate -- renovate-config-validator --strict                                                                                                         ~/dev/uv
(node:770) [DEP0040] DeprecationWarning: The `punycode` module is deprecated. Please use a userland alternative instead.
(Use `node --trace-deprecation ...` to show where the warning was created)
 INFO: Validating .github/renovate.json5
 INFO: Config validated successfully
```


---

_Merged by @AlexWaygood on 2024-05-07 16:09_

---

_Closed by @AlexWaygood on 2024-05-07 16:09_

---

_Branch deleted on 2024-05-07 16:09_

---

_Label `internal` added by @AlexWaygood on 2024-05-07 16:10_

---
