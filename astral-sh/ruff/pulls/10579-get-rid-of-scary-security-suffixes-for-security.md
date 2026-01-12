```yaml
number: 10579
title: Get rid of SCARY SECURITY suffixes for security-related renovate PRs
type: pull_request
state: merged
author: AlexWaygood
labels: []
assignees: []
merged: true
base: main
head: scary-security-prs
created_at: 2024-03-25T17:45:48Z
updated_at: 2024-03-25T21:34:42Z
url: https://github.com/astral-sh/ruff/pull/10579
synced_at: 2026-01-12T15:55:32Z
```

# Get rid of SCARY SECURITY suffixes for security-related renovate PRs

---

_@AlexWaygood_

Just use a `security` label instead

---

_Comment by @AlexWaygood on 2024-03-25 17:47_

See e.g. the PR title in https://github.com/astral-sh/ruff/pull/10577 for what we're trying to avoid... docs here: https://docs.renovatebot.com/configuration-options/#vulnerabilityalerts

---

_Review requested from @zanieb by @AlexWaygood on 2024-03-25 17:47_

---

_@zanieb approved on 2024-03-25 17:56_

:) thanks

---

_Merged by @AlexWaygood on 2024-03-25 17:57_

---

_Closed by @AlexWaygood on 2024-03-25 17:57_

---

_Branch deleted on 2024-03-25 17:57_

---

_@T-256 reviewed on 2024-03-25 21:29_

---

_Review comment by @T-256 on `.github/renovate.json5`:45 on 2024-03-25 21:29_

This repo doesn't have `security` label. renovatebot couldn't add this label to its PRs.

---

_@AlexWaygood reviewed on 2024-03-25 21:34_

---

_Review comment by @AlexWaygood on `.github/renovate.json5`:45 on 2024-03-25 21:34_

I'm pretty sure it would have just created one, but I've created one just to be sure: https://github.com/astral-sh/ruff/labels/security

---
