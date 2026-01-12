```yaml
number: 11324
title: Tell renovate not to try to update GitHub runners
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
assignees: []
merged: true
base: main
head: renovate-macos
created_at: 2024-05-07T15:47:17Z
updated_at: 2024-05-07T16:02:36Z
url: https://github.com/astral-sh/ruff/pull/11324
synced_at: 2026-01-12T15:55:37Z
```

# Tell renovate not to try to update GitHub runners

---

_@AlexWaygood_

## Summary

If we've pinned a GitHub runner to a specific version (e.g. `macos-12` rather than `macos-latest`, we've probably done so for a specific reason, so PRs like https://github.com/astral-sh/ruff/pull/11300 are just annoying. The fix for this is a bit obscure, but it appears the way to do it is to setup a package rule that only matches renovate updates which use the `github-runners` datasource, and then set `enabled: false` in that package rule.

Docs:
- https://docs.renovatebot.com/configuration-options/#matchdatasources
- https://docs.renovatebot.com/configuration-options/#enabled

## Test Plan

```
% npx --yes --package renovate -- renovate-config-validator --strict                                               ~/dev/ruff
(node:57154) [DEP0040] DeprecationWarning: The `punycode` module is deprecated. Please use a userland alternative instead.
(Use `node --trace-deprecation ...` to show where the warning was created)
 INFO: Validating .github/renovate.json5
 INFO: Config validated successfully
```


---

_Label `ci` added by @AlexWaygood on 2024-05-07 15:50_

---

_@zanieb approved on 2024-05-07 15:51_

---

_Comment by @zanieb on 2024-05-07 15:52_

This would be nice in uv too.

---

_Comment by @AlexWaygood on 2024-05-07 15:52_

> This would be nice in uv too.

yup, coming right up

---

_Comment by @charliermarsh on 2024-05-07 15:57_

Thank you!

---

_Merged by @AlexWaygood on 2024-05-07 16:02_

---

_Closed by @AlexWaygood on 2024-05-07 16:02_

---

_Branch deleted on 2024-05-07 16:02_

---
