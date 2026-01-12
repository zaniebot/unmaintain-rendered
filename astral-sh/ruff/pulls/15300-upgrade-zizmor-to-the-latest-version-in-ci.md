```yaml
number: 15300
title: Upgrade zizmor to the latest version in CI
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
assignees: []
merged: true
base: main
head: alex/upgrade-zizmor
created_at: 2025-01-06T12:44:24Z
updated_at: 2025-01-06T15:07:48Z
url: https://github.com/astral-sh/ruff/pull/15300
synced_at: 2026-01-12T15:55:50Z
```

# Upgrade zizmor to the latest version in CI

---

_@AlexWaygood_

## Summary

This PR upgrades zizmor to the latest release in our CI. zizmor is a static analyzer checking for security issues in GitHub workflows. The new release finds some new issues in our workflows; this PR fixes some of the issues, and adds ignores for some other issues.

The issues fixed in this PR are new cases of zizmor's [`template-injection`](https://woodruffw.github.io/zizmor/audits/#template-injection) rule being emitted. The issues I'm ignoring for now are all to do with the [`cache-poisoning`](https://woodruffw.github.io/zizmor/audits/#cache-poisoning) rule. The main reason I'm fixing some but ignoring others is that I'm confident fixing the template-injection diagnostics won't have any impact on how our workflows operate in CI, but I'm worried that fixing the cache-poisoning diagnostics could slow down our CI a fair bit. I don't mind if somebody else is motivated to try to fix these diagnostics, but for now I think I'd prefer to just ignore them; it doesn't seem high-priority enough to try to fix them right now :-)

## Test Plan

- `uvx pre-commit run -a --hook-stage=manual` passes locally
- Let's see if CI passes on this PR...


---

_Label `ci` added by @AlexWaygood on 2025-01-06 12:44_

---

_Marked ready for review by @AlexWaygood on 2025-01-06 13:30_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2025-01-06 13:30_

---

_@charliermarsh approved on 2025-01-06 15:05_

---

_Merged by @AlexWaygood on 2025-01-06 15:07_

---

_Closed by @AlexWaygood on 2025-01-06 15:07_

---

_Branch deleted on 2025-01-06 15:07_

---
