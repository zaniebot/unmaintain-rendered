```yaml
number: 21719
title: Use OIDC instead of codspeed token
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: micha/token-free-codspeed
created_at: 2025-12-01T07:21:10Z
updated_at: 2025-12-01T16:51:36Z
url: https://github.com/astral-sh/ruff/pull/21719
synced_at: 2026-01-10T16:48:02Z
```

# Use OIDC instead of codspeed token

---

_Pull request opened by @MichaReiser on 2025-12-01 07:21_

Don't use a persistent token, instead use OIDC to generate a token on the fly, as recommended by codspeed.

I also updated the `mode` from `instrumentation` to `simulation` because codspeed logged a deprecation warning

---

_Label `ci` added by @MichaReiser on 2025-12-01 07:21_

---

_Review requested from @woodruffw by @MichaReiser on 2025-12-01 07:28_

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 07:28_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Marked ready for review by @MichaReiser on 2025-12-01 07:28_

---

_@woodruffw approved on 2025-12-01 15:39_

Nice! Just flagging for diligence that `CODSPEED_TOKEN` should be removed from the actions secrets once this is merged too 🙂 

---

_Merged by @MichaReiser on 2025-12-01 16:51_

---

_Closed by @MichaReiser on 2025-12-01 16:51_

---

_Branch deleted on 2025-12-01 16:51_

---
