```yaml
number: 1599
title: Add the ability to trigger property tests on a PR by adding a label to the PR
type: issue
state: open
author: AlexWaygood
labels:
  - ci
  - testing
assignees: []
created_at: 2025-11-20T11:53:15Z
updated_at: 2025-11-20T11:53:15Z
url: https://github.com/astral-sh/ty/issues/1599
synced_at: 2026-01-12T15:54:25Z
```

# Add the ability to trigger property tests on a PR by adding a label to the PR

---

_@AlexWaygood_

It would be nice to be able to trigger property tests on a PR by adding a label to the PR, similar to the way we do it with ecosystem-analyzer. They're too slow to run on every PR by default, but you generally want to run them on any PR that touches our `Type::has_relation_to_impl` logic, and it's annoying to have to check out the PR locally in order to run them.

---

_Label `ci` added by @AlexWaygood on 2025-11-20 11:53_

---

_Label `testing` added by @AlexWaygood on 2025-11-20 11:53_

---
